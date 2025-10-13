# join-pdf

https://jimvella.github.io/join-pdf/

Join PDF documents by loading into the webpage and then 'Save as PDF'.

Note that printing the webpage rather than concatenating at the PDF format level avoids PDF interoperability issues, however is does result in a significantly larger file size since the process effectively converts the PDF data into images.

## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```sh
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```sh
npm run build
```

You can preview the production build with `npm run preview`.

## Deploying

Deployed via GitHub pages https://github.com/jimvella/join-pdf/deployments

Running the build step updates the files in /docs. Commit the changes and push to GitHib to deploy.
