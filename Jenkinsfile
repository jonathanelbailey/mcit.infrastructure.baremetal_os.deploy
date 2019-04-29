node {
    // checkout scm and assign its var maps to scm_output variable for future use
    stage("checkout scm") {
        checkout(
            [$class: 'GitSCM', branches: [[name: '**']],
            doGenerateSubmoduleConfigurations: false,
            extensions: [[$class: 'WipeWorkspace'],
            [$class: 'LocalBranch']],
            submoduleCfg: [],
            userRemoteConfigs: [[credentialsId: '8e92e71b-b9a2-4a58-aba1-4b98b3b7666b',
            url: 'https://github.com/jonathanelbailey/mcit.infrastructure.baremetal_os.deploy.git']]]
        )
    }
    stage('Deploy OS') {
        withCredentials([usernamePassword(
            credentialsId: 'ca75d291-93c4-47f8-aa5a-3a3b0b703d9c',
            passwordVariable: 'sudo_pass',
            usernameVariable: 'sudo_user')]
        ){
            wrap([$class: 'AnsiColorBuildWrapper', colorMapName: "xterm"]) {
                ansiblePlaybook(
                    playbook: "$env.WORKSPACE/deploy_os.yml",
                    inventory: "$env.WORKSPACE/hosts",
                    credentialsId: 'ca75d291-93c4-47f8-aa5a-3a3b0b703d9c',
                    hostKeyChecking: false,
                    colorized: true,
                    extraVars: [
                        ansible_become_pass: [ value: "$sudo_pass", hidden: true ]
                    ]
                )
            }
        }
    }