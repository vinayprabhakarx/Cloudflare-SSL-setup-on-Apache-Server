# Cloudflare-SSL-setup-on-Apache-Server

## **Generating SSL Certificate with Cloudflare**

### Step 1: Connect Your Domain with Cloudflare

 **a. Login to Cloudflare:**
   - Visit the Cloudflare website and log in to your account.

**2. Add and Link Your Domain:**
   - In the Cloudflare dashboard, click on "Add a Site" to add and link your domain.
   - Follow the instructions to complete the domain setup process.

### Step 2: Configure SSL/TLS Settings

- **Navigate to SSL/TLS Settings:**
   - In the Cloudflare dashboard, go to the SSL/TLS section.

- **Choose SSL/TLS Mode:**
   - Under the SSL/TLS settings, select the appropriate SSL/TLS mode. "Flexible," "Full," or "Full (strict)" depending on your needs.

### Step 3: Generate SSL Certificate

**a. Access Origin Server Settings:**
   - Within the SSL/TLS settings, navigate to the "Origin Server" tab.

**b. Create Certificate:**
   - Click on the "Create Certificate" button.

**c. Fill Certificate Details:**
   - Enter the necessary information for your SSL certificate, including the domain name.
   - Choose the encryption level and cipher suite according to your requirements.

**d. Generate Private Key and CSR:**
   - Cloudflare will generate a private key and Certificate Signing Request (CSR).
   - Copy the both `SSL` and `private key` securely

**e. Configure SSL on Your Server:**
   - Install the generated SSL certificate and private key on your origin server.


## Configure SSL on Your Apache Server

<H3><span style="color: #f0c571;">Prerequisites:</span></H5>

- Apache web server installed and running.
- Root or sudo access to the server.
- A valid SSL certificate and private key.

**Step 1: Create Cloudflare Configuration Directory**
```bash
sudo mkdir -p /etc/cloudflare
```

**Step 2: Navigate to Cloudflare Configuration Directory**
```bash
cd /etc/cloudflare
```

**Step 3: Create and Edit SSL Certificate File**
```bash
sudo vim your_domain.com.pem
```
Enter `SSL certificate` that you copy from clouflare and save the file.

<span style="color: #fa7878;">**Note:**</span>
Make sure to replace `your_domain.com` with your actual `domain`.



<span style="color: #89fa78;">**Vim Editor Note**</span>
- **Insert Mode:** Press `i`
- **Exiting Insert Mode:** Press `Esc`
- **Save and Exit:** type `:wq`
- **Exit Without Saving:** type `:q!`



**Step 4: Create and Edit Private Key File**
```bash
sudo vim your_domain.com.key
```
Enter `private key` that you copy from clouflare and save the file.

**Step 5: Enable SSL Module**
```bash
sudo a2enmod ssl
```

**Step 6: Navigate to Apache Configuration Directory**
```bash
cd /etc/apache2/sites-available
```

**Step 7: Copy Default Configuration for Your Domain**
```bash
sudo cp 000-default.conf your_domain.com.conf
```

**Step 8: Edit Your Domain's Configuration File**
```bash
sudo vim your_domain.com.conf
```
Enter the following configuration:
```apache
<VirtualHost *:443>
    ServerAdmin webmaster@your_domain.com
    ServerName your_domain.com
    ServerAlias www.your_domain.com
    DocumentRoot /var/www/html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    SSLEngine on
    SSLCertificateFile /etc/cloudflare/your_domain.com.pem
    SSLCertificateKeyFile /etc/cloudflare/your_domain.com.key
</VirtualHost>
```

**Step 9: Test Configuration File**
```bash
apachectl configtest
```
**Step 10: Enable Your Domain's SSL Configuration**
```bash
sudo a2ensite your_domain.com.conf
```

**Step 11: Reload Apache Server**
```bash
sudo systemctl reload apache2
```
