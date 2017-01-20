@Library('portia-resources') import com.portia.shared.Azure

node {
    def MSBuild = "${tool 'MSBuild'}\\MSBuild.exe"
    stage('Checkout') { // for display purposes
        git url: 'https://github.com/PortiaSA/corporateServices.git'

    }
    stage('Package Restore'){
        bat("nuget restore Services\\Statistics\\PS3\\Statistics.sln")
    }
    stage('Build'){
        bat("\"${MSBuild}\" Services\\Statistics\\PS3\\Statistics.sln /p:Configuration=Production /p:TargetProfile=Production  /target:Clean;Rebuild")
    }
    stage('Unit Test'){
        //bat returnStatus: true, script: "packages\\xunit.runner.console.2.1.0\\tools\\xunit.console.exe \"Services\\Statistics\\PS3\\Statistics.Tests\\Statistics.AcceptanceTests.v1\\bin\\Production\\Portia.CorporateServices.Statistics.AcceptanceTests.v1.dll\" -xml report.xml"

        /*step([$class: 'XUnitBuilder',
              thresholds: [[$class: 'FailedThreshold', unstableThreshold: '1']],
              tools: [[$class: 'XUnitDotNetTestType', pattern: 'report.xml']]])*/
    }
    stage('Deploy'){
        def azure = new Azure(".\\", "Services\\Statistics\\PS3\\Statistics.Web", "statistics-api", "(stagingslot)", Azure.DeployType.MSDeploy, "WebDeployPackage\\Statistics.Web.csproj.zip")
        azure.publish()
    }
}