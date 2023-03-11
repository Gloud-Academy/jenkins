def html = '<html><head><title>Pipeline Status Report</title></head><body>'
def jobs = Jenkins.instance.getAllItems(AbstractProject.class)
jobs.each { job ->
  if (job.class.canonicalName == "org.jenkinsci.plugins.workflow.job.WorkflowJob") {
    def lastBuild = job.getLastBuild()
    html += "<h1>${job.displayName}</h1><table><tr><th>Build Number</th><th>Status</th><th>Duration</th></tr>"
    while (lastBuild != null) {
      def number = lastBuild.getNumber()
      def result = lastBuild.getResult().toString()
      def duration = lastBuild.getDurationString()
      html += "<tr><td><a href='${job.getUrl()}${number}'>#${number}</a></td><td>${result}</td><td>${duration}</td></tr>"
      lastBuild = lastBuild.getPreviousBuild()
    }
    html += "</table>"
  }
}
html += '</body></html>'
def reportFile = new File("${JENKINS_HOME}/report.html")
reportFile.write(html)
emailReport(reportFile)

void emailReport(File reportFile) {
  emailext (
    to: 'harish_raj@gove.co',
    subject: 'Pipeline Status Report',
    body: 'Please find attached the daily pipeline status report.',
    attachLog: true,
    attachmentsPattern: "${reportFile.getAbsolutePath()}"
  )
}
