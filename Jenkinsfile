node {
   
withCredentials([sshUserPrivateKey(credentialsId: 'Remote-Server-Access-Creds', keyFileVariable: 'PASSWORDLESS', passphraseVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
	
properties([parameters([choice(choices: ['site1', 'site2', 'site3'], description: 'Select site for building the application', name: 'sitename'), gitParameter(branch: '', branchFilter: '.*', defaultValue: 'origin/master', description: '', name: 'BRANCH', quickFilterEnabled: false, selectedValue: 'NONE', sortMode: 'NONE', tagFilter: '*', useRepository: 'git@github.com:ritesh-globant/sample-drupal-application.git', type: 'PT_BRANCH'), choice(choices: ['UAT-192.168.1.6', 'Dev-192.168.1.7'], description: 'Select the server for deploying the application', name: 'servers')])])
	
site = params.sites.trim()
String[] server = params.servers.trim().split("-");
	
branchname = params.BRANCH.trim()
def branch = params.BRANCH.trim().replaceAll("origin/","")
	
def defaultPath = "cd /var/www/application/web/"
	
def themesfolder
if ( site == 'site1') {
   themesfolder = "${defaultPath}themes/custom/site1" 
} else if(site == 'site2') {
  themesfolder = "${defaultPath}themes/custom/site2"
} else {
  themesfolder = "${defaultPath}themes/custom/site3"
}
	
def cacheClear = "${defaultPath}; drush -l ${site} cr"
def enableMaintenance = "${defaultPath}; drush -l ${site} sset system.maintenance_mode 1"
def gitpull = "${defaultPath}; git pull origin ${branch}"
def composer = "${defaultPath}../; composer install"
def updateDB = "${defaultPath}; drush -l ${site} updb -y"
def importConfig = "${defaultPath}; drush -l ${site} cim -y"
def disableMaintenance = "${defaultPath}; drush -l ${site} sset system.maintenance_mode 0"
def nodeInstall = "${themesfolder}; npm install"
def gulp = "${themesfolder}; gulp"
    
def remote = [:]
remote.name = server[0]
remote.host = server[1]
remote.allowAnyHosts = true
remote.user = USERNAME
remote.identityFile = PASSWORDLESS
remote.passphrase = PASSWORD
    
stage('Clear Cache'){	
sshCommand remote: remote, command: cacheClear
}

stage('Enable Maintenance Mode'){	
sshCommand remote: remote, command: enableMaintenance	
}

stage('Git Pull'){	
sshCommand remote: remote, command: gitpull
}

stage('Composer'){	
sshCommand remote: remote, command: composer	
}

stage('Update DB'){	
sshCommand remote: remote, command: updateDB
}
	
stage('Import Config'){	
sshCommand remote: remote, command: importConfig	
}

stage('Disable Maintenance Mode'){	
sshCommand remote: remote, command: disableMaintenance
}

stage('Node Install'){	
sshCommand remote: remote, command: nodeInstall	
}

stage('Gulp'){	
sshCommand remote: remote, command: gulp	
}

stage('Clear Cache'){	
sshCommand remote: remote, command: cacheClear	
 }

}	

}
