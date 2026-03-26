# Migration Guide: UpCloud to Cloudflare Pages

This guide outlines the steps to migrate the GeeksHacking static website from its current UpCloud instance to Cloudflare Pages.

## 1. Prerequisites

Before starting the migration, ensure you have:
*   Access to the GeeksHacking Cloudflare account.
*   Admin access to the GitHub repository containing the website code.
*   Access to the domain registrar (where `geekshacking.com` is registered) to update DNS records.

## 2. Setting up Cloudflare Pages

1.  Log in to your Cloudflare Dashboard.
2.  Navigate to **Workers & Pages** in the left sidebar.
3.  Click **Create application**, then select the **Pages** tab.
4.  Click **Connect to Git**.
5.  Select **GitHub** and authorize Cloudflare to access your repositories.
6.  Select the `GeeksHacking` repository.
7.  Configure the build settings:
    *   **Project name:** Choose a name (e.g., `geekshacking-website`).
    *   **Production branch:** Typically `master` or `main`.
    *   **Framework preset:** Select `None` (since this is a plain HTML/CSS/JS site).
    *   **Build command:** Leave empty.
    *   **Build output directory:** Leave empty (defaults to the root directory where `index.html` is located).
8.  Click **Save and Deploy**. Cloudflare Pages will build and deploy your site to a temporary `.pages.dev` subdomain.

## 3. Addressing Server-Side Code (`mailer.php`)

**Important:** Cloudflare Pages only hosts static files (HTML, CSS, JS, images). It **does not** support running PHP scripts like `mailer.php`.

The contact form in `index.html` currently seems to be commented out, but if you need a functional contact form, you will have to replace `mailer.php` with a serverless solution.

**Options for replacing `mailer.php`:**

1.  **Formspree or similar services:**
    *   Sign up for Formspree (or Netlify Forms, getform.io, etc.).
    *   They provide an endpoint URL.
    *   Update the `<form>` tag in `index.html` to point to the new endpoint: `<form action="https://formspree.io/f/your_endpoint" method="POST">`
    *   You can completely remove `mailer.php` from the repository.
2.  **Cloudflare Workers:**
    *   Create a Cloudflare Worker to act as the backend for your form.
    *   The worker can process the POST request from the form and send an email using an API like SendGrid or Mailgun.
    *   This keeps everything within the Cloudflare ecosystem but requires more setup.

## 4. Configuring the Custom Domain

Once the site is successfully deployed to the `.pages.dev` subdomain and you've verified it works:

1.  In your Cloudflare Pages project dashboard, go to the **Custom domains** tab.
2.  Click **Set up a custom domain**.
3.  Enter your domain (e.g., `geekshacking.com`).
4.  If your domain's DNS is already managed by Cloudflare, it will automatically add the necessary CNAME records.
5.  If your DNS is managed elsewhere (or via UpCloud), Cloudflare will provide you with a CNAME record that you must add to your DNS provider. It will look like:
    *   **Type:** CNAME
    *   **Name:** `geekshacking.com` (or `@`)
    *   **Target:** `geekshacking-website.pages.dev`
6.  Once the DNS propagates, your website will be live on Cloudflare Pages.

## 5. Cleaning Up

1.  **Remove GitHub Action:** Delete the `.github/workflows/deploy.yml` file. Cloudflare Pages automatically handles deployments when you push to the repository, so the old SSH-based deployment to UpCloud is no longer needed.
2.  **Decommission UpCloud Server:** Once you have verified the site is fully functional on Cloudflare Pages (including the new contact form solution, if any) and the DNS has fully propagated, you can safely shut down and delete the UpCloud instance to save costs.