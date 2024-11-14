pipeline
{
    agent any

    environment 
    {
        IMAGE_NAME = 'rishikeshavs/netflix-clone'
        DOCKER_HUB_CREDENTIALS = '2276428b-546e-4735-b3df-66bc4e8f1da0'
        DOCKER_BUILDKIT = "1" // Enables Docker BuildKit
        TMDB_V3_API_KEY = "4f1a32d7b72a66deed43fc83e0636dce" //API KEY
    }

    stages
    {
        stage('Cloning into CodeBase')
        {
            steps
            {
                git branch: 'main', url: 'https://github.com/rishikeshav2715/Netflix-Clone-App.git'
            }
        }

        stage('Build docker image')
        {
            steps
            {
                script
                {
                    def tagged_image = "${IMAGE_NAME}:${env.BUILD_ID}"

                    sh """
                    docker build --build-arg TMDB_V3_API_KEY="${env.TMDB_V3_API_KEY}" -t tagged_image .
                    """
                    writeFile file: 'build_id.txt', text: "${env.BUILD_ID}"
                    sh 'ls -la'
                }
            }
        }

        stage('Save Build ID')
        {
            steps
            {
                script
                {
                    archiveArtifacts artifacts: 'build_id.txt', fingerprint: true, followSymlinks: false, onlyIfSuccessful: true
                }
            }
        }

        stage('Push to DockerHub')
        {
            steps
            {
                script
                {
                    docker.withRegistry('',"${DOCKER_HUB_CREDENTIALS}")
                    {
                        docker.image("${IMAGE_NAME}:${env.BUILD_ID}").push()
                    }
                }
            }
        }
    }
}