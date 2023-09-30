# mailcow-config

# Configuring a Mailcow Server on Linux with Domain Integration

This guide will help you set up a Mailcow email server on a Linux machine and integrate it with your domain. Follow the steps below to get started.

## Step 1: Install Docker

```bash
# Update package list
apt update

# Install Docker and Docker Compose
apt install docker-compose
```
## Step 2: Install Mailcow
```
# Navigate to the /opt directory
cd /opt

# Clone the Mailcow repository
git clone https://github.com/mailcow/mailcow-dockerized

# Enter the Mailcow directory
cd mailcow-dockerized
```

## Step 3: Configuring Postfix
To configure Postfix for your Mailcow server, follow these steps:

1. Open a terminal on your Linux machine.

2. Navigate to the Mailcow directory:

```
bash
cd /opt/mailcow-dockerized
```

3. Edit the `extra.cf` file:

```
nano data/conf/postfix/main/extra.cf
```

4. Add the following parameters to `extra.cf`: 

```
# Set the hostname of your server as follows:
myhostname = mail.iadmexico.mx
```
```
# Set the supported IP protocol (in this case, it is IPv4 to avoid reverse DNS):
inet_protocols = ipv4
```
```
# Configure the relay host, which should be pointed to the desired relay (in this case, we're using Google):
relayhost = [smtp-relay.gmail.com]:587
```
```
# Set the virtual transport, indicating the default delivery transport:
virtual_transport = smtp:[smtp-relay.gmail.com]:25
```

5: Save the `extra.cf` file and exit the text editor, also restart the postfix service.
```
 docker compose restart postfix-mailcow
```

## Starting mailcow
```
# Pull Mailcow images
docker-compose pull

# Start Mailcow containers
docker-compose up -d
```

## Mailcow Configuration

1. Access the Mailcow web interface by visiting `<mail.domain>/admin` in your web browser.

2. Navigate to the "Routing" tab and add a "Sender-dependent transport" with the following settings:

   - Host: [smtp-relay.gmail.com]:587
   - Username and password: Leave them empty

    - Sample image
    ![image](https://github.com/neximo/mailcow-config/assets/116591665/db6160dd-6d4b-4d8b-a635-69710215319d)


3. Add your domain:

   - Description: Any description that fits your configuration
   - Tags: Leave empty
   - Sender-dependent transports: Select the one created in the "Routing" tab
   - Relay options:
     - Relay this domain: Unchecked
     - Relay all recipients: Unchecked
     - Relay non-existing mailboxes only: Unchecked
   - Active: Checked
   -Leave all other settings with their default values.
   - Sample image
   ![image](https://github.com/neximo/mailcow-config/assets/116591665/c4d95195-7695-443e-a1fb-7313d023cc57)


## Google (GSuite) Configuration

### Step 1: Host Configuration (https://admin.google.com/ac/apps/gmail/hosts)

Specify the name as "Single host" and use your full domain `<mail.iadmexico.mx>` with port 25.

![image](https://github.com/neximo/mailcow-config/assets/116591665/12b81822-d125-4bf9-a374-ccaf31990851)


### MX Records Configuration (https://admin.google.com/ac/apps/gmail/setup)

Ensure all Google SMTP settings are correctly configured.

![image](https://github.com/neximo/mailcow-config/assets/116591665/bc36a1e9-37f9-4278-9ac0-9746f38d862e)


### Routing Configuration (https://admin.google.com/ac/apps/gmail/routing)


#### Routing

*Screenshot 1.*

![image](https://github.com/neximo/mailcow-config/assets/116591665/ac5aba99-7fb8-4076-b6f5-a0c552285a4c)

*Screenshot 2.*

![image](https://github.com/neximo/mailcow-config/assets/116591665/e1becf1c-fa41-4e9d-bfb3-9e90b6baa906)

*Screenshot 3.*

![image](https://github.com/neximo/mailcow-config/assets/116591665/9de7f9ba-9746-4466-9457-e9acb1d314e3)

*Screenshot 4.*

![image](https://github.com/neximo/mailcow-config/assets/116591665/f3985ae7-de0a-44e6-bb2c-fcc9eafe2d31)


*Screenshot 1 second routing.*

![image](https://github.com/neximo/mailcow-config/assets/116591665/6fa7b4e6-0b6e-4630-925a-3da546183d89)

*Screenshot 2 second routing.*

![image](https://github.com/neximo/mailcow-config/assets/116591665/c6ee40ef-b690-489e-be01-83cc9729bd6f)

*Screenshot 3 second routing.*

![image](https://github.com/neximo/mailcow-config/assets/116591665/429b15db-53d6-4c26-a342-96246fdec4d3)





#### Relay service

*Screenshot 1.*
![image](https://github.com/neximo/mailcow-config/assets/116591665/1d6429a3-c507-4b6b-b3cc-6e9e31aa2036)


*Screenshot 2.*
![image](https://github.com/neximo/mailcow-config/assets/116591665/128fb51c-7ee3-42cb-acf2-689344148e07)

## Testing

You can test with local emails to ensure that the server is redirecting them, you can also track the emails that were received by the Google relay 

https://admin.google.com/ac/emaillogsearch




