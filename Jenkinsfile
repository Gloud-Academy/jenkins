node {
    stage('Consolidated Report') {
        def jobs = Jenkins.instance.getAllItems(Job.class)
        def report = ""
        for (job in jobs) {
            if (job.getClass().getName() == "org.jenkinsci.plugins.workflow.job.WorkflowJob") {
                def build = job.getLastBuild()
                report += "Pipeline: ${job.name}, Status: ${build.result}\n"
            }
        }
        echo(report)
    }
}
