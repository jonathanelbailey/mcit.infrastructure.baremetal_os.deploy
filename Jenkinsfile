node {
    // checkout scm and assign its var maps to scm_output variable for future use
    stage("checkout scm") {
        checkout(
            [$class: 'GitSCM', branches: [[name: '**']],
            doGenerateSubmoduleConfigurations: false,
            extensions: [[$class: 'WipeWorkspace'],
            [$class: 'LocalBranch']],
            submoduleCfg: [],
            userRemoteConfigs: [[credentialsId: 'github',
            url: 'https://github.com/jonathanelbailey/mcit.infrastructure.baremetal_os.deploy.git']]]
        )
    }
    stage('Deploy OS') {
        withCredentials([usernamePassword(
            credentialsId: 'hp_ilo',
            passwordVariable: 'sudo_pass',
            usernameVariable: 'sudo_user')]
        ){
            wrap([$class: 'AnsiColorBuildWrapper', colorMapName: "xterm"]) {
                ansiblePlaybook(
                    playbook: "$env.WORKSPACE/deploy_os.yml",
                    inventory: "$env.WORKSPACE/hosts",
                    // credentialsId: 'hp_ilo',
                    hostKeyChecking: false,
                    colorized: true,
                    extraVars: [
                        ansible_become_pass: [ value: "$sudo_pass", hidden: true ]
                    ]
                )
            }
        }
    }
}