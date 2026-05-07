# Static Webhosting On Azure and Integrate CI/CD Using GitHub Actions
 
Host a static website on Azure Storage, accelerate it globally with Azure CDN, and automate every deployment with GitHub Actions.

 ![Azure Static Hosting](https://user-images.githubusercontent.com/66474973/235602607-de6e6668-db18-4fa0-9d9c-a209fbe81681.png)


## Architecture
 
![Architecture Diagram](https://raw.githubusercontent.com/manoop98/images/main/Screenshot%202026-05-07%20104650.png)
 
The flow has two halves:
 
- **CI/CD pipeline** (top): A push to the repository triggers a GitHub Actions workflow that authenticates with Azure and uploads the site files to the storage account's `$web` container.
- **Azure infrastructure** (bottom): Azure CDN sits in front of the storage account, caching content at edge POPs around the world and serving end users with low latency.

## How it works
 
1. A developer pushes a commit to the `main` branch on GitHub.
2. The GitHub Actions workflow checks out the code, logs into Azure using a service principal, and uploads the files to the `$web` container.
3. The CDN endpoint — whose origin points at the storage account's static-website endpoint — serves cached files to users from the nearest edge.
4. The workflow purges the CDN cache after each deploy so visitors get the latest version.

## Prerequisites
 
- An active Azure subscription
- A GitHub account
- Azure CLI installed locally (for one-time setup)
- Static site files (HTML / CSS / JS) in this repository

 
## Repository structure
 
```
.
├── index.html              # Site entry point
├── 404.html                # Custom 404 page
├── assets/                 # CSS, JS, images
└── .github/
    └── workflows/
        └── deploy.yml      # CI/CD workflow
```
 
## Accessing the site
 
- **Storage endpoint** (origin): `https://<yourstorageaccount>.z13.web.core.windows.net`
- **CDN endpoint** (production URL): `https://<yourcdnendpoint>.azureedge.net`
Use the CDN URL as the public-facing address.
 
## Cost notes
 
- Storage account on `Standard_LRS` is the cheapest tier and is more than enough for static assets.
- Azure CDN `Standard_Microsoft` charges per GB egressed and per request — typically pennies per month for low-traffic sites.
- GitHub Actions provides 2,000 free minutes per month on public repositories.
 


