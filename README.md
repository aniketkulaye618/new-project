# new-project

Create Four ec2 instance and install packages as required: 

developer
jenkins
ansible
web-server

passwordless ssh required between : 
jenkins to ansible
ansible to webserver

*************************************************************************************************************************************

GITHUB

create one repositery in github and make one file index.html

Now integrate github to my jenkins server:

copy jenkins URL
setting > webhook > Add webhook > paste jenkins URL /github-webhook/ > secrtes(u need to genrate new token) 

go to users configure > add token > genrate token > copy and paste it to secrtes > Add webhook   (In jenkins will get)

*************************************************************************************************************************************
Jenkins

Now you need to enable plugin name as "publish over ssh", it helps u to connect to ansible servers,
means whaterver the compile and bulid output it will trasfer to ansible server and also help to run playbook in ansible server.

Install git on jenkins server

create one project in jenkins

give parameters accroding to your requirements:

Bulid tigger : Github hook tigger for GITScms polling (it will commit directly whenever developer chnages their code)

Bulid enviroment : None 

Bulid: send file or execute command over ssh
(here we need to add server, and give command that will be excute in server)
1st we need to add  jenkins servers for ssh login
Manage jenkins > configure system > go to last line (ADD SSH SERVERS) 
Now go to "send file or execute command over ssh" and add below command that will be excute in jenkins server
rsync -avh /var/lib/jenkins/workbook/demo-project/*.html root@ansibleserver:/opt/

Post Bulid action : 
Now it will copy the file from ansible server to webserver
1st we need to add ansible servers for ssh login to excute command
Manage jenkins > configure system > go to last line (ADD ansible SSH SERVERS) 
Now go to Post Bulid action > "send bulid artifacts over ssh" and add below command that will be excute in ansible server
ansible-playbook /sourcecode/playbook.yml


***************************************************************************************************************************************

Ansible

mkdir /sourcecode/

vi playbook.yml

- hosts: all
  tasks:
      - copy:
	       src: /opt/index.html
		   dest: /var/www/html
		   
***************************************************************************************************************************************   

Developer System

yum install git

git clone repositery URL

after that it will show u directoy for that project and all source codes inside that directory

chnages source code:

vi index.html 

git commit -m "f1" index.html

git push origin master ---> code commit karke git server pe push kar raha hai --> new job will run 

git push -u origin main


https://github.com/aniketkulaye618/new-project.git
