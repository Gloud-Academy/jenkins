import hudson.model.*
import jenkins.*
import groovy.text.*

// Define report file path and email recipients
def reportPath = "E:\local-server\jenkins\report.html"
def recipients = ["harish_raj@gove.co"]

// Retrieve list of jobs
def jobs = Jenkins.instance.getAllItems(Job.class)

// Loop through jobs and generate HTML report
def reports = jobs.collect { job ->
    def lastBuild = job.getLastBuild()
    def builds = []
    while (lastBuild != null) {
        builds << [
            number: lastBuild.getNumber(),
            result: lastBuild.getResult().toString(),
            timestamp: new Date(lastBuild.getStartTimeInMillis()),
            duration: lastBuild.getDurationString()
        ]
        lastBuild = lastBuild.getPreviousBuild()
    }
    [jobName: job.getName(), report: template.toString()]
}

// Combine all reports into a single file
def reportFile = new File(reportPath)
reportFile.withWriter { writer ->
    writer.write('<html><head><title>Jenkins Build Report</title></head><body>')
    reports.each { report ->
        writer.write('<h1>' + report.jobName + '</h1>')
        writer.write(report.report)
    }
    writer.write('</body></html>')
}

// Send report via email
Jenkins.instance.sendMail(
    subject: 'Jenkins Build Report - ' + new Date().format('MM/dd/yyyy'),
    body: reportFile.text,
    mimeType: 'text/html',
    to: recipients.join(",")
)
