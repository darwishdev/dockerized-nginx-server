## Setting Up a Second Nginx Instance for Your Django App (with Frappe ERPNext)

This repository provides a step-by-step guide on deploying your Django application alongside an existing Frappe ERPNext installation. We'll achieve this by setting up a separate Nginx instance using Docker, ensuring isolated configuration and smooth operation for both applications.

**Prerequisites:**

- A cloud server (AWS, GCP, DigitalOcean, etc.) with SSH access.
- Basic understanding of Docker concepts.

**Steps:**

1. **Clone the Repository:**

   Open your terminal and ssh to your server. Then, run the following command to clone this repository:

   ```bash
   git clone https://github.com/darwishdev/dockerized-nginx-server.git
   ```
 

2. **Configure Your Project:**

   Navigate to the project directory:

   ```bash
   cd src/apps/omdabus.esolvelabs.com
   ```

   **Important:**

   - Update your Django project's `settings.py` file. Replace all occurrences of `omdabus.esolvelabs.com` with your actual domain name.

3. **Generate SSL Certificates:**

   **a. Cloudflare Token:**

   - Create a Cloudflare API token with "Zone: Edit" permissions for the domain you want to use.
   - Save the token in the `src/nginx/dns-credentials/cloudflare.ini` file, replacing `yourtoken` with your actual token:

     ```
     dns_cloudflare_api_token=yourtoken
     ```

   **b. Generate Certificates:**

   - Run the following command in your terminal (replace `yourdomain.com` with your actual domain):

     ```bash
     certbot certonly --dns-cloudflare --dns-cloudflare-credentials dns-credentials/cloudflare.ini -d app.yourdomain.com --email info@yourdomain.com --agree-tos --non-interactive --force-renewal --cert-name app.yourdomain.com --keep-until-expiring --rsa-key-size 4096
     ```

   This command will generate SSL certificates using Certbot and Cloudflare DNS validation.

4. **Run the Containers:**

   - **Optional:** Edit the configuration files (`nginx/conf.d/your_domain.conf`) if you need to further customize Nginx settings. Replace the placeholder domain names with your actual domain.

   - Build and run the Docker containers in detached mode:

     ```bash
     cd src/appps/omdabus.esolvelabs.com;\
     docker-compose up -d;\
     cd src/common/nginx;\
     docker-compose up -d
     ```

5. **Access Your Django App:**

   Open your web browser and visit `http://your_domain:81` (replace `your_domain` with your actual domain). You should now see your Django application running!

**Additional Resources:**

- For more detailed instructions and explanations, refer to the original article: [Link to the article](https://medium.com/p/384e379f018e)

By following these steps, you'll have a separate Nginx instance serving your Django application alongside your existing Frappe ERPNext installation. This approach ensures optimal performance and avoids conflicts between the two applications.
