Prereqs:
Jenkins server with access to install tools onto it or you can use the neighboring Dockerfile

Why would you need this:
* You want to demo work to clients so they can approve things in a UAT environment
* You want to run functional, uat, or other tests

Steps:
- Install ddev-live CLI tool(link to install docs), jq
- Configure your Jenkins credential manager to include your ddev-live-token
- Configure a new Jenkins job credential binding to use the token
- Add the following or similarly modified for your needs to a Execute Shell command
```
eval $(/usr/share/jenkins/ref/.linuxbrew/bin/brew shellenv)

SITENAME=staging-${GIT_BRANCH#*/}-$BUILD_NUMBER
ddev-live auth --token=$TOKEN --default-org=your-org

ddev-live create site drupal $SITENAME --github-repo=you/your-repo --branch=${GIT_BRANCH#*/}
/usr/share/jenkins/ref/devrel/jenkins-ddev-live/wait_site_healthy.sh $SITENAME

url=$(ddev-live describe site ${SITENAME} -o json | jq -r .previewUrl)
/usr/share/jenkins/ref/devrel/jenkins-ddev-live/wait_curl_healthy.sh $url
```
- Run the build and wait to see your new branch launched into DDEV-Live
- Delete the site when you're done with it
