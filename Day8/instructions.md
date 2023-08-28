You can access these instructions any time via "Exam Information".


Remote Desktop
In June 2022 the Exam UI changed from a Remote Terminal to a Remote Desktop (Ubuntu XFCE). You're only allowed to use the Firefox browser inside the Remote Desktop to access the K8s Docs, not your local browser. Coming from OSX/Windows there will be changes in copy&paste. There is an article from us describing the new UI and the Linux Foundation released an update. There is also a playground which allows you to test a similar UI.

There are still differences between the real exams and the simulators, like:

You'll need to use the PSI Secure Browser to access the real exams whereas you can use Chrome/Firefox to access the simulators
You can only access certain allowed URLs in the Remote Desktop's Firefox of the real exams whereas there are no such restrictions in the simulators
In the simulators you can use the top navigation Exam Interface to switch back to the old Remote Terminal in case of issues (let us know of any problems on Support), this isn't possible in the real exams

Countdown / Timer
The 120 minutes countdown will start once the session is started. It will be shown on top left. The countdown is just an just an indicator for yourself and won't revoke access to your environment when finished.

Solutions & Score
When the countdown reaches 0 you'll be able to see the proposed solutions for each task and your score. You can also access these earlier by selecting "Exam Controls -> Answers & Score". But it's recommended to try solving by yourself at first.

36hrs Access
You'll have access to your simulator environment during the next 36hrs. You'll always have access to the questions and solutions.

Browser reload or close
For the 36hrs your session will be kept running in the background. You can close the window or even use a different browser without losing changes.

Permissions
You have root permissions using sudo should you encounter any permission issues.

Keyboard issues (some keys won't work)
For OSX it could be your keyboard layout. Please go to Preferences->Keyboard->Input Sources. There search for English and then select "ABC" (or maybe called "Default"). Switching the layout should solve the input issue.
For Windows try to change your keyboard layout to a different English one.
For Ubuntu+Chrome for example users report keyboard issues, switch to Firefox or Chromium. Also open killer.sh in private browser mode.

Rules
You're only allowed to have one other browser tab open with:

https://kubernetes.io/docs
https://kubernetes.io/blog
https://helm.sh/docs
Notepad
Use Application->Accessories->Mousepad to write down notes for yourself during the exam.

Difficulty
This simulator is more difficult than the real certification. We think this gives you a greater learning effect and also confidence to score in the real exam. Most of the simulator scenarios require good amount of work and can be considered "hard". In the real exam you will also face these "hard" scenarios, just less often. There are probably also fewer questions in the real exam.

Make sure
Only edit/modify/delete those resources you want to act on. In most scenarios there are existing resources which could be part of other questions. Just like in a real world cluster.

Content
All simulator sessions of the same type (CKS|CKA|CKAD) will have identical content/scenarios.

Slow or interrupted connection?
If you experience any kind of issues please make sure all points here are complied with:

Browser: only latest Chrome and latest Firefox are supported
Ubuntu+Chrome: users report keyboard issues, switch to Firefox or Chromium
Extensions: disable ALL extensions/plugins and run in private mode
VPN/Proxy: don't use a VPN/Proxy
Internet: use a stable internet connection, with low usage by others
Help / Support
FAQ for answers
Slack for scenario discussions
Support for session/account issues



## Qs:

Task weight: 5%


Team Mercury asked you to perform some operations using Helm, all in Namespace mercury:

Delete release internal-issue-report-apiv1
Upgrade release internal-issue-report-apiv2 to any newer version of chart bitnami/nginx available
Install a new release internal-issue-report-apache of chart bitnami/apache. The Deployment should have two replicas, set these via Helm-values during install
There seems to be a broken release, stuck in pending-install state. Find it and delete it

A:
helm uninstall internal-issue-report-apiv1 --namespace mercury

helm repo update

helm upgrade internal-issue-report-apiv2 bitnami/nginx --namespace mercury

helm install internal-issue-report-apache bitnami/apache --set replicaCount=2 --namespace mercury

helm list --namespace mercury --all


Task weight: 2%

Team Neptune needs a Job template located at /opt/course/3/job.yaml. This Job should run image busybox:1.31.0 and execute sleep 2 && echo done. It should be in namespace neptune, run a total of 3 times and should execute 2 runs in parallel.

Start the Job and check its history. Each pod created by the Job should have the label id: awesome-job. The job should be named neb-new-job and the container neb-new-job-container.

Task weight: 2%

Create a single Pod of image httpd:2.4.41-alpine in Namespace default. The Pod should be named pod1 and the container should be named pod1-container.

Your manager would like to run a command manually on occasion to output the status of that exact Pod. Please write a command that does this into /opt/course/2/pod1-status-command.sh. The command should use kubectl.

Task weight: 1%

The DevOps team would like to get the list of all Namespaces in the cluster. Get the list and save it to /opt/course/1/namespaces.

Task weight: 4%

There is an existing Deployment named api-new-c32 in Namespace neptune. A developer did make an update to the Deployment but the updated version never came online. Check the Deployment history and find a revision that works, then rollback to it. Could you tell Team Neptune what the error was so it doesn't happen again?

Task weight: 4%

Team Neptune has its own ServiceAccount named neptune-sa-v2 in Namespace neptune. A coworker needs the token from the Secret that belongs to that ServiceAccount. Write the base64 decoded token to file /opt/course/5/token.

Task weight: 3%

Team Sunny needs to identify some of their Pods in namespace sun. They ask you to add a new label protected: true to all Pods with an existing label type: worker or type: runner. Also add an annotation protected: do not delete this pod to all Pods having the new label protected: true.

A:
k -n sun get pod --show-labels
kubectl label pods -n sun -l type=worker protected=true
kubectl label pods -n sun -l type=runner protected=true
kubectl annotate pods -n sun -l protected=true protected="do not delete this pod"

Task weight: 4%

There seems to be an issue in Namespace mars where the ClusterIP service manager-api-svc should make the Pods of Deployment manager-api-deployment available inside the cluster.

You can test this with curl manager-api-svc.mars:4444 from a temporary nginx:alpine Pod. Check for the misconfiguration and apply a fix.

## Tips

export do="--dry-run=client -o yaml"    # k create deploy nginx --image=nginx $do

export now="--force --grace-period 0"   # k delete pod x $now

https://editor.networkpolicy.io/?id=BZoS8jz3AxAwfgOY for network policies