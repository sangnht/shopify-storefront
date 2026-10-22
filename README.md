# Shopify Theme CI/CD Setup Guide

This document provides instructions on how to obtain the necessary information (`STORE_NAME`, `THEME_ID`, `SHOPIFY_CLI_THEME_TOKEN`) to configure an automated deployment (CI/CD) pipeline for your Shopify theme using GitHub Actions.

---

## 1. How to get the Store Name (`STORE_NAME`)

The `STORE_NAME` variable is the unique identifier for your Shopify store.

1. Log in to your Shopify Admin panel.
2. Look at your browser's address bar (URL). 
3. Extract the root domain name of your store, excluding `https://` and any trailing paths.
   - **Example URL:** `https://admin.shopify.com/store/sangnht-dev-store`
   - **STORE_NAME Value:** `sangnht-dev-store.myshopify.com`

---

## 2. How to get the Theme ID (`THEME_ID`)

You need to get the Theme ID to declare it for the **Development** (Test theme) and **Production** (Live theme) environments.

1. From your Shopify Admin panel, go to **Online Store** > **Themes**.
2. Find the theme you want to deploy code to (either a draft theme or the Live theme).
3. Click the **three dots (...)** icon next to that theme and select **Edit code**.
4. Look at the browser's address bar (URL); you will see a long sequence of numbers at the end of the URL.
   - **Example URL:** `admin.shopify.com/store/sangnht-dev-store/themes/1357924680`
   - **THEME_ID Value:** `1357924680`
5. Save this ID for your respective Staging and Production environments.

---

## 3. How to get the Authentication Token (`SHOPIFY_CLI_THEME_TOKEN`)

Depending on your store type, there are two ways to obtain the deployment token. 

### Option A: For Live / Standard Stores (Via Custom App)
If you are deploying to a standard active store, you can generate a token directly from the Admin settings.

1. Go to **Settings** > **Apps and sales channels** > **Develop apps**.
2. Click **Allow custom app development** (if it's your first time), then click **Create an app**.
3. Name the app (e.g., "GitHub CI/CD").
4. Go to the **Configuration** tab and click **Configure** in the **Admin API integration** section.
5. Scroll down or search for **Themes**, then check both `read_themes` and `write_themes`. Click **Save**.
6. Switch to the **API credentials** tab and click **Install app**.
7. Once installed, click **Reveal token once** in the *Admin API access token* section. 
8. **Copy this token immediately** (it usually starts with `shpat_...`). This is your `SHOPIFY_CLI_THEME_TOKEN`.

### Option B: For Development Stores (Via Theme Access App)
*Note: Newer Development Stores restrict direct Custom App creation. Use this official workaround.*

1. Go to the [Shopify App Store](https://apps.shopify.com/) and search for the **Theme Access** app (or click [here](https://apps.shopify.com/theme-access)).
2. Click **Install** to add the app to your store.
3. Once installed, open the **Theme Access** app from your left-hand admin menu.
4. Click on the **Create password** button.
5. Enter the necessary email information to grant access and confirm.
6. The system will generate a password string (usually starting with `shptka_...`). 
7. **Copy this password immediately**. This is your `SHOPIFY_CLI_THEME_TOKEN`.

---

## 4. How to configure GitHub Actions Secrets

Once you have gathered the three pieces of information above, add them to your GitHub Secrets by following these steps:

1. Go to your Repository on GitHub.
2. Navigate to **Settings** > **Environments**.
3. Create two new environments (Click the *New environment* button):
   - `Development` (For testing)
   - `Production` (For the live store - *remember to enable "Required reviewers" here to enforce manual approval before deployment*)
4. Inside each Environment setting, scroll down to the **Environment secrets** section and click **Add secret**.
5. Add the three variables with their corresponding values obtained in the previous steps:
   - `SHOPIFY_CLI_THEME_TOKEN`
   - `STORE_NAME`
   - `THEME_ID`

Now, every time you push code to the `master` branch, the CI/CD workflow will run automatically and safely deploy your code to your Shopify store.
