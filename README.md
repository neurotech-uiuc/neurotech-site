# neurotech-site

_Documentation and how-to for everything related to the NeurotechX student chapter at UIUC website._

[Link to the site!](http://neurotech.web.illinois.edu)

## Implementation Choice

We obviously have the intention to make the site as low cost as possible. Using UIUC resources, the cost can be completely free for a basic site, which should be plenty for the purposes of Neurotech at UIUC branding for a good amount of time.

### Current Implementation

We have chosen to use [cPanel](https://answers.illinois.edu/illinois/82587) to host our website. It comes with a `.web.illinois.edu` address, and we can eventually contact IT services to request a different domain name. cPanel comes with quite a few capabilities, and most can be viewed in the home portal.

As of Fall 2019, we are currently building the site using Jekyll, which uses a combination of HTML, sass, markdown/kramdown and Liquid. Information on all of these languages is provided in the Neurotech Google Drive under "Outeach/Reference! NT's Website Crash Course".

### Future Implementation

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
2. Click on the settings button all the way to the right of the desired member, then click `Change Role...` (this can only be done once they have accepted the invitation)
3. Choose `Owner`

Note that owners to the organization have admin access to all repositories in the organization as well. Defined hierarchy/security within the organization is not urgent (or arguably necessary), but can promote good git workflow regarding PRs, etc.

#### Repository

For as long as the organization has the default repository permission set to write, every member of the organization will have write access to the website's repo. Only owners of the organization have admin access to this repository, and only admins may currently override PR review requirements (into master). This will most likely be changed (to including admins needing review) when the site is more finalized.

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

The deploy commands will most likely not need to be changed often, especially once the site is mostly finalized.

**Important:** The directory structure on the server must match the file structure on master. For example, if the repository has a `css` directory and the server does not, even a deploy file that looks like this:

```yml
    - /bin/cp css/* $DEPLOYPATH/css/
```

will not create the directory. Instead, the deploy will (most likely) fail. Changing what commands are run on deploy are covered in the next section.

Deploy steps:

1. Make sure that your locally checked out version of the master branch website functions as expected.
2. From the cPanel home portal, click on `Git Version Control` under "FILES".
3. There should only be one repository listed (neurotech-site); click on `Manage` to the right of it.
4. Click on the `Pull or Deploy` tab at the top.
5. Click `Update from Remote`. Make sure that the commit messages (and commit sha if messages are not unique) to confirm you have the correct version of the master branch.
6. Click `Deploy HEAD Commit`. If the deploy succeeds, then the master sha's and commit messages under "Last Deployment Information" will match what's under "HEAD Commit". Otherwise, the deploy has failed, and the previous version of the website is still (most likely) active.

### Deploy Command Maintenance

The only command in the `.cpanel.yml` file is to run a script that is stored locally on the sever. In order to flip different options within the script, you must edit it on the server to either comment the lines you don't want run, or uncomment the lines you do want run. There should be comments above each block of commands describing what they do and when they should and shouldn't be run. Although it is technically possible to run a script with arguments that could selectively run these blocks for us (instead of having to comment/uncomment), this would require editing the `.cpanel.yml` file on GitHub to include those arguments before every deploy OR not deploying through the GUI on cPanel at all but doing it through terminal. The downside to the second option is that there will not be a log or record for deploy times and versions.

This script should not need to be edited aside from commenting and uncommenting except in the cases where directory structure or our website infrastruture (requiring additional or different compilation commands) has changed.

If the directory structure has changed, make changes to the `make-dirs.sh` script to reflect the full, correct directory structure.

## Environment Setup (Jekyll)

### cPanel

These steps shouldn't need to be taken again, but are here just for the purpose of history/documentation.
- created Ruby 2.2 app 
- downloaded Ruby 2.5 since Jekyll is not supported on 2.2
- `gem install jekyll bundler` which failed
- `gem install jekyll`
- `gem install bundler`
- `gem install jekyll bundler`
- `gem install bigdecimal` since jekyll didn't want to succeed on running anything without it

To use any ruby commands (i.e. `gem`, `jekyll`, `bundle`) you need to enter the ruby environment via the command: `source /home/neurotech/rubyvenv/.ruby-jekyll/2.5/bin/activate`

### Local

**Windows:**
- WSL: you will need some bash CLI, if you have Windows 10, I would recommend WSL. [Here](https://docs.microsoft.com/en-us/windows/wsl/install-win10) is the Microsoft documentation on how to do that.
- Jekyll: if you just downloaded WSL, you will eventually learn how frustrating it is. If you've already had it, then you probably understand the struggle. Here are the steps you need to take to get it working (these steps derived from [here](https://www.mczerniawski.pl/blog/run-jekyll-localy-in-wsl/), credit where credit is due):
```
sudo apt-get update && sudo apt-get upgrade
```
The above line might take a while, especially if you just downloaded WSL.
```
sudo apt-get install ruby
sudo gem install bundler
```
You will need to create a `Gemfile` now which can be done by typing the command `cat > Gemfile`, typing the desired contents, then pressing Ctl-D. This is what should be in `Gemfile`:
```
source "https://rubygems.org"
gem "jekyll-include-cache"
gem "jekyll-paginate"
gem "jekyll-sitemap"
gem "jekyll-gist"
gem "jekyll-feed"
```
Now, continuing on with the commands:
```
sudo apt-get install ruby-all-dev zlib1g-dev libxslt1-dev libxml2-dev
sudo gem install commonmarker
bundle install
```

**Mac:**

Mac is not the OS I run, so using Homebrew is only my recommendation but also the only method I'm going to document here. If you have something else, you probably already know what you're doing and can find out how to get Jekyll working locally.

So to install Homebrew if you don't have it:
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
To get ruby installed and added to your shell:
```
brew install ruby
export PATH=/usr/local/opt/ruby/bin:$PATH
```
To [install jekyll](https://jekyllrb.com/docs/installation/macos/#install-jekyll):
```
gem install --user-install bundler jekyll
export PATH=$HOME/.gem/ruby/X.X.0/bin:$PATH
```
To make sure the install worked, type `jekyll help` and you should get a list of the different jekyll commands you can run.

**Linux:**

Literally google any tutorial, you got this.
