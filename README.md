# React Native Expo and Neon's Serverless Postgres Platform

This repository provides an example of how to use GitHub Actions to generate a preview build for a React Native Expo application. It also demonstrates how to create a corresponding Neon database branch.


## Prerequisites

Before we begin, you will need to have a few things already setup.

- An [Expo account](https://expo.dev/signup) for deploying and testing preview branches.
- A [Neon account](https://console.neon.tech/signup) with with a database project for branching.
- A clone of this repository,

## Setting up GitHub

On your GitHub repository page, navigate to `Settings` → `Secrets and variables` → `Actions` and add the following secrets and variables.

- **Neon API Key:**  Obtain a Neon API key from the `API Keys`  by clicking on your avatar in the upper right corner, then `Account Settings` → `API Keys`. Once you have created your API key, add it to your GitHub projects `Repository Secrets` with the name `NEON_API_KEY`.
- **Neon Project ID:** The Neon Project ID can be obtained from your Neon project’s `Settings` → `General` page. Add the Neon Project ID to your GitHub projects `Repository Variables` under then name `NEON_PROJECT_ID`.
- **Neon Database Username:** Add the DB Role to be used for branch creation to your GitHub projects `Repository Variables` under then name `NEON_USERNAME`.
- **Expo Token:** Obtain an Expo access token by going to `Account Settings` → `Access tokens`. Then add a `Robot user` by clicking on `Add robot` and giving it the name `GITHUB_ACTIONS`. Next, click on `Create token` and give it a descriptive, meaningful name, something like `GitHub actions token for PR builds`. Finally, copy the token and add it to your GitHub projects `Repository Secrets` with the name `EXPO_TOKEN`.

## Setting up your database

Go to the SQL Editor for your database project in the [Neon Console](https://console.neon.tech/app/projects/) and run the SQL statements below.

```SQL
CREATE TABLE neon_app_content(id SERIAL PRIMARY KEY, text_field TEXT NOT NULL, value TEXT);
INSERT INTO neon_app_content(text_field, value) VALUES ('name', 'Ryan')
```

## Testing it out

To test it all out, perform the following steps:
1. Create a development branch.
2. Change the word `Welcome` on line 37 of the ./app/(tabs)/index.tsx file to `Greetings`.
3. Commit your changes and push the branch to your repository.
4. Create a pull request for your development branch into main.
5. Once the GitHub Action completes, open the preview build on your mobile device using the QR code in the comments of the Pull Request. You should an image like below.

![<img src="/images/initial_preview.jpeg" height="250" alt="React Native Expo application with text greetings Ryan"/>](/images/initial_preview.jpeg)


6. Go back to the SQL Editor in the Neon console and run the following SQL statement against the database branch for your PR. It should be named something like this `preview/pr-4-development_branch_name`.

```SQL
UPDATE neon_app_content
SET value='Program'
WHERE text_field = 'name'
```
7. Reload the app on your mobile device by using a 3-finger press on the app, and selecting `Reload`. You should now see the name changed from `Ryan` to `Program`.

![React Native Expo application with text greetings program](/images/initial_preview.jpeg)