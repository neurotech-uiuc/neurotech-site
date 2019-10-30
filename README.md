# neurotech-site

_Documentation and how-to for everything related to the NeurotechX student chapter at UIUC website._

## Implementation Choice

We obviously have the intention to make the site as low cost as possible. Using UIUC resources, the cost can be completely free for a basic site, which should be plenty for the purposes of NeurotechX UIUC branding for a good amount of time.

### Current Implementation

We have chosen to use [cPanel](https://answers.illinois.edu/illinois/82587) to host our website. It comes with a `.web.illinois.edu` address, and we can eventually contact IT services to request a different domain name. cPanel comes with quite a few capabilities, and most can be viewed in the home portal.

As of Fall 2019, we are in the very early stages of development on the website, so we will currently only be using plain html and css. This should be sufficient for most of the website, but moving on to and implementing other languages will help with future organization of the website and provide the possibility for additional capabilities.

HTML and CSS should work out of the box for pretty much anyone's local development environment, so additional steps for installation of required dependencies/languages/libraries will be included here once they are implemented.

### Future Implementation

The first future implementation to be implemented is Sass. This should happen before or soon after the first rollout of the website, and its realized implementation is dependent upon cPanel allowing the installation of Dart Sass and being able to compile on deploy.

Since it looks like cPanel supports Ruby, the fastest way to support the blog feature of the website may be to use something like Jekyll. This needs to be looked into more, but is a possible future implementation. Other options for the blog feature of the app include Wordpress, which can also be integrated into cPanel.

More in the future, it would be nice to implement Ruby (and possibly Ruby on Rails) since it is available with cPanel. Node.js is also configurable with cPanel, but Ruby is often less code (for a site that doesn't have very complex aspects such as this one), and RoR will play nicely if we decide to implement a database, which also comes with cPanel; a database might be useful for purposes of the blog (like logging in or editing posts), managing applications for the organization, event details, etc. Of course, deciding whether to use Node or Ruby in the future can still be looked into.

## Administrative

(Regarding permissions, etc.)

### Github

#### Organization

To add a new member to the organization (assuming you are an organization admin):

1. Navigate to the organization page
2. Click on the `People` tab at the top, then on the right in green, click `Invite Member`
3. Search however necessary for the person you wish to add (they must have a GitHub account)
4. Once they accept the invite, you can control their permissions and organization role from the `People` tab. They are by default a member, and there are only two levels of roles, Member and Owner.

To make someone an owner **_to the organization_** (most likely for a new lead):

1. Again visit the `People` tab
2. Click on the settings button all the way to the right of the desired member, then click `Manage`.
3. ??? Profit

Note that owners to the organization have admin access to all repositories in the organization as well. Defined hierarchy/security within the organization is not urgent (or arguably necessary), but can promote good git workflow regarding PRs, etc.

#### Repository

For as long as the organization has the default repository permission set to write, every member of the organization will have write access to the website's repo. Only owners of the organization have admin access to this repository, and only admins may currently override PR review requirements (into master). This will most likely be changed (to including admins needing review) when the site is more finalized or has been released.

To change the organization's default repositroy permissions so that not all members have write access to every other repository:

1. Navigate to the settings of the organization page.
2. Click on the `Member privileges` tab on the menu to the left-hand side.
3. Change the drop down under "Base permissions" from `Write` to the desired setting.

To make it so that repository admins can no longer override required PR reviews:

1. Navigate to the `Settings` tab at the top of the repository.
2. Click on the `Branches` tab on the menu to the left-hand side.
3. Under "Branch protection rules" click `Edit` next to master. (This is also where you can add and manage all branch protection rules.)
4. Tick the box labeled "Include administrators", near the bottom.

### cPanel

To give someone permission to cPanel:

1. Navigate to the cPanel home portal.
2. Under "COMMONLY USED FEATURES", click on `Manage cPanel Access`.
3. Enter the person's email address, then click `Add` to the right. The person should immediately be able to login and have access using [this link](https://go.illinois.edu/cPanelLogin).

## Deployment

### Deploying Current Version of `master`

You will first want to make sure that the deploy commands are updated to reflect all necessary changes made to the structure of the repository. The deploy commands will most likely not need to be changed often, especially once the site is mostly finalized.

**Important:** The directory structure on the server must match the file structure on master. For example, if the repository has a `css` directory and the server does not, even a deploy file that looks like this:

```yml
    - /bin/cp css/* $DEPLOYPATH/css/
```

will not create the directory. Instead, the deploy will fail. You can easily edit the file structure through cPanel's terminal (under "ADVANCED") with basic bash commands.

Deploy steps:

1. Make sure that your locally checked out version of the master branch website functions as expected.
2. From the cPanel home portal, click on `Git Version Control` under "FILES".
3. There should only be one repository listed (neurotech-site); click on `Manage` to the right of it.
4. Click on the `Pull or Deploy` tab at the top.
5. Click `Update from Remote`. Make sure that the commit messages (and commit sha if messages are not unique) to confirm you have the correct version of the master branch.
6. Click `Deploy HEAD Commit`. If the deploy succeeds, then the master sha's and commit messages under "Last Deployment Information" will match what's under "HEAD Commit". Otherwise, the deploy has failed, and the previous version of the website is still (most likely) active.

### Deploy Commands

Refrain from copying everything (`*`) from the repo onto the server in order to save space. Copy over only what is needed.

Looking at the current `.cpanel.yml` file, you can probably make out how exactly it works; the commands listed are the ones that one when you click the button to deploy. Currently, all these commands do is copy over files from where the repository is in the cPanel to the root directory for the web server. Eventually, these commands might include additional steps, such as compiling Sass down to CSS.
