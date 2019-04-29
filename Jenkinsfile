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
            passwordVariable: 'hp_ilo_pass',
            usernameVariable: 'hp_ilo_user')]
        ){
            wrap([$class: 'AnsiColorBuildWrapper', colorMapName: "xterm"]) {
                ansiblePlaybook(
                    playbook: "$env.WORKSPACE/deploy_os.yml",
                    inventory: "$env.WORKSPACE/hosts",
                    // credentialsId: 'hp_ilo',
                    hostKeyChecking: false,
                    colorized: true,
                    extraVars: [
                        hp_ilo_user: [ value: "$hp_ilo_user" ],
                        hp_ilo_pass: [ value: "$hp_ilo_pass", hidden: true ]
                    ]
                )
            }
        }
    }
}