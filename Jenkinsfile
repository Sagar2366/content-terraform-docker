node {
  git url 'https://github.com/Sagar2366/content-terraform-docker.git'
  if(action == 'Deploy') {
    stage('init') {
        bat """
            terraform init
        """
    }
    stage('plan') {
      bat label: 'terraform plan', script: "terraform plan -out=tfplan -input=false -var image_name=${image_name} -var ext_port=${ext_port}"
      bat {
          timeout(time: 10, unit: 'MINUTES') {
              input(id: "Deploy Gate", message: "Deploy environment?", ok: 'Deploy')
          }
      }
    }
    stage('apply') {
        bat label: 'terraform apply', script: "terraform apply -lock=false -input=false tfplan"
    }
  }

  if(action == 'Destroy') {
    stage('plan_destroy') {
      bat label: 'terraform plan destroy', script: "terraform plan -destroy -out=tfdestroyplan -input=false -var image_name=${image_name} -var ext_port=${ext_port}"
    }
    stage('destroy') {
      bat {
          timeout(time: 10, unit: 'MINUTES') {
              input(id: "Destroy Gate", message: "Destroy environment?", ok: 'Destroy')
          }
      }
      bat label: 'Destroy environment', script: "terraform apply -lock=false -input=false tfdestroyplan"
    }
  }
}
