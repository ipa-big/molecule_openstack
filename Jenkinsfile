def platforms = [
    "ubuntu2404",
    "ubuntu2204",
    "ubuntu2004",
    "ubuntu1804",
    "centos10",
    "centos9",
    "centos8",
    "centos7",
    "debian13",
    "debian12",
    "debian11"
]

def parallel_stages = [:]

for (p in platforms) {
    def platform = p
    parallel_stages[platform] = {
        docker
            .image("${MOLECULE_DOCKER_IMAGE}")
            .inside("--name ${JOB_NAME}_${platform} -e OS_AUTH_URL=${OS_AUTH_URL} -e OS_USERNAME=${OS_APPLICATION_CREDENTIAL_ID} -e OS_PASSWORD=${OS_APPLICATION_CREDENTIAL_SECRET} -u root") {

            stage("Prepare node") {
                sh "rm -r ~/.ansible/roles/ipa-big.molecule_openstack"
                sh "ansible-playbook molecule/default/prepare.yml"
            }

            stage("Molecule Converge") {
                sh "molecule -vv converge -s ${platform}"
            }

            stage("Molecule Verify") {
                sh "molecule -vv verify -s ${platform}" //  || (echo 'sleep for 300s' && sleep 300)
            }

            stage("Molecule Destroy") {
                sh "molecule -vv destroy -s ${platform}"
            }
        }
    }
}

node {
    checkout scm

    withCredentials([usernamePassword(
            credentialsId: 'openstack-credentials',
            usernameVariable: 'OS_APPLICATION_CREDENTIAL_ID',
            passwordVariable: 'OS_APPLICATION_CREDENTIAL_SECRET'
    )]) {
        stage("Pull molecule image") {
            sh "docker pull ${MOLECULE_DOCKER_IMAGE}"
        }

        parallel(parallel_stages)
    }
}




@NonCPS
List<List<?>> mapToList(Map map) {
    return map.collect { it ->
        [it.key, it.value]
    }
}