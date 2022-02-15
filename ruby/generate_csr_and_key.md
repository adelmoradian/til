# Generate CSR and key

```ruby
require 'openssl'

def gen_key(name)
  key = OpenSSL::PKey::RSA.new 1048
  file = File.new(name, "w")
  file.write(key)
  file.close
end

def get_key(name)
  OpenSSL::PKey::RSA.new File.open(name)
end

def csr(key)
  options = { 
  
  :country      => 'PL',
  :state        => 'M',
  :city         => 'Cracow',
  :organization => 'OSPL',
  :department   => '', 
  :common_name  => 'OSPL',
  :email        => ''
  
  }

  
  request = OpenSSL::X509::Request.new
  request.version = 0 
  request.subject = OpenSSL::X509::Name.new([
  ['C',             options[:country], OpenSSL::ASN1::PRINTABLESTRING],
  ['ST',            options[:state],        OpenSSL::ASN1::PRINTABLESTRING],
  ['L',             options[:city],         OpenSSL::ASN1::PRINTABLESTRING],
  ['O',             options[:organization], OpenSSL::ASN1::UTF8STRING],
  ['OU',            options[:department],   OpenSSL::ASN1::UTF8STRING],
  ['CN',            options[:common_name],  OpenSSL::ASN1::UTF8STRING],
  ['emailAddress',  options[:email],        OpenSSL::ASN1::UTF8STRING]
  
  ])  
  request.public_key = key.public_key
  request.sign(key, OpenSSL::Digest::SHA1.new)
end 

def check_csr(request)
   csr = OpenSSL::X509::Request.new request
   rais 'CSR can not be verified' unless csr.verify csr.public_key
end

def sign_csr(request, key)
  name = OpenSSL::X509::Name.parse 'CN=ospl/DC=example'

  csr_cert = OpenSSL::X509::Certificate.new
  csr_cert.serial = 0
  csr_cert.version = 2
  csr_cert.not_before = Time.now
  csr_cert.not_after = Time.now + 600
  csr_cert.subject = request.subject
  csr_cert.public_key = request.public_key
  csr_cert.issuer = name
  csr_cert.sign key, OpenSSL::Digest::SHA1.new
end

def public_encrypt(cert,data)
  cert.public_encrypt data
end

def private_encrypt(cert,data)
  cert.private_encrypt data
end

def public_decrypt(cert,data)
   cert.public_decrypt data
end

def private_decrypt(cert,data)
   cert.private_decrypt data
end

def test
   p "Create server and user key ..."
   gen_key 'server.key'
   gen_key 'user.key'

   p "Load server and user key ..."
   user_key = get_key 'user.key'
   server_key = get_key 'server.key'

   p "Create user csr..."
   user_csr = csr user_key

   p "Check user csr ..."
   check_csr user_csr

   p 'Sign user csr by server ...'
   signed_user_csr = sign_csr user_csr, server_key

   p "Encrypt message by server ... "
   encrypted_data = public_encrypt signed_user_csr.public_key, "Top secret from server message"
   p encrypted_data
   p "========== end ==========="

   p "Decrypt messsage by user ... "
   p private_decrypt user_key, encrypted_data
   p "========== end ==========="

   p "Encrypt message by user ... "
   p encrypted_from_user = private_encrypt( user_key, "Top secret from user")
   p "========== end ==========="

   p "Decrypt message by server ... "
   p public_decrypt signed_user_csr.public_key, encrypted_from_user
   "========== end ==========="
end
```
