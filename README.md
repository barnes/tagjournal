# Tag Journal (tagjournal)

A simple, fast journal app.

## Install the dependencies
```bash
npm install
```

### Start the app in development mode (hot-code reloading, error reporting, etc.)
```bash
quasar dev
```

### Lint the files
```bash
npm run lint
```

### Build the app for production
```bash
quasar build
```

### Customize the configuration
See [Configuring quasar.conf.js](https://v1.quasar.dev/quasar-cli/quasar-conf-js).


### Build Log
#### 03/29/21
Starting with authorization with firebaseui. Installed, and visable, but hella ugly. Need to clean it up.

Styling functional now. Need to break into a Login component. Header will have Sign In if not logged in and 'My Journal' & 'Sign Out' if signed in. 

Able to read if a user is signed in or not. Need to get a user collection created on new user still. Pretty close to getting that up and running, just having issues getting the db object to sync up.

Users are being added to the DB. Logout function is working. Getting user status is working. The bones are there, just need to clean it up with conditional rendering for logged in and not logged in users.

#### 03/30/21
Conditional header navigation is done. Login / log out workflow is functional. Pages all made. Last hiccup is this damn delayed rendering of the login UI. Must restart page to get it to load. Probably some funky issue with the way quasar / vue loads the pages. Once I can work out that issue, lock down this branch as a nice and easy drop in authentication boilerplate.

#### 3/31/21
Login page renders correctly now. Finally. All ready to rock and roll. 

#### 4/1/21
Journal Entry Page allows for creation of tags, still need to have buttons add or remove the tag from their journal entry. 


