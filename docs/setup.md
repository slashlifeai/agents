# Hosting DIDs on GitHub Pages with Cloudflare

This document provides a step-by-step guide on how to host your Decentralized Identifiers (DIDs) on GitHub Pages and configure a custom domain using Cloudflare.

## Prerequisites

* A GitHub account.
* A domain name (e.g., `slashlife.ai`).
* A Cloudflare account.

## Step 1: Create a GitHub Repository

1. Create a new public GitHub repository. For this example, we'll name it `agents`.
2. Clone the repository to your local machine.

```bash
git clone https://github.com/<your-username>/agents.git
cd agents
```

## Step 2: Add Your DID Documents

1. Create the necessary files and directories for your DIDs. The structure should look like this:

```
.
├── .well-known
│   ├── hana
│   │   └── did.json
│   ├── max
│   │   └── did.json
│   └── zeoy
│       └── did.json
├── CNAME
├── README.md
├── deno.json
└── index.html
```

2. The content of the `did.json` files should be the DID documents you've created.
3. The `CNAME` file should contain your custom domain name, e.g., `agents.slashlife.ai`.
4. The `index.html` file can be a simple page listing the DIDs.
5. The `deno.json` file can be used to serve the files locally for testing.

## Step 3: Push to GitHub

Commit and push your changes to the GitHub repository.

```bash
git add .
git commit -m "Initial commit with DID documents"
git push origin main
```

## Step 4: Enable GitHub Pages

1. Go to your repository's settings on GitHub.
2. In the "Code and automation" section of the sidebar, click **Pages**.
3. Under "Build and deployment", under "Source", select **Deploy from a branch**.
4. Under "Build and deployment", under "Branch", use the `main` branch and `/ (root)` folder, then click **Save**.
5. GitHub Pages will now build and deploy your site. It will be available at `https://<your-username>.github.io/agents/`.

## Step 5: Configure Cloudflare

1. **Add your domain to Cloudflare:**
   - Log in to your Cloudflare account.
   - Click on "+ Add a site" and enter your root domain (e.g., `slashlife.ai`).
   - Select the free plan.
   - Cloudflare will scan for existing DNS records.

2. **Update your nameservers:**
   - Cloudflare will provide you with two nameservers.
   - Go to your domain registrar's website and update the nameservers for your domain to the ones provided by Cloudflare.
   - This process may take some time to propagate.

3. **Configure DNS records:**
   - In your Cloudflare dashboard, go to the "DNS" section.
   - Add a `CNAME` record for your subdomain (`agents`).
   - Point the `CNAME` record to your GitHub Pages URL: `<your-username>.github.io`.
   - Make sure the "Proxy status" is set to "DNS only" (the orange cloud should be grey). This is important for GitHub Pages to be able to verify your custom domain.

   The record should look like this:

   | Type    | Name   | Content                  | Proxy status |
   |---------|--------|--------------------------|--------------|
   | CNAME   | agents | `<your-username>.github.io` | DNS only     |

4. **Configure GitHub Pages for your custom domain:**
   - Go back to your repository's settings on GitHub.
   - In the "Code and automation" section of the sidebar, click **Pages**.
   - Under "Custom domain", enter your full custom domain (`agents.slashlife.ai`) and click **Save**.
   - GitHub will check your DNS settings. Once it's verified, it will automatically enable HTTPS for your custom domain.

## Step 6: Verify Your DIDs

Your DIDs should now be accessible via your custom domain:

* `https://agents.slashlife.ai/.well-known/max/did.json`
* `https://agents.slashlife.ai/.well-known/hana/did.json`
* `https://agents.slashlife.ai/.well-known/zeoy/did.json`

You can also visit `https://agents.slashlife.ai` to see the `index.html` page.
