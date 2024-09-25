pipeline {
    agent any


    stages {
        stage('Generate Idle Jobs Report') {
            steps {
                script {
                    def report = getIdleJobsReport()

                    // Generate the HTML report with clickable job names
                    def reportOutput = """
                    <html>
                    <head>
                        <style>
                            body { font-family: Arial, sans-serif; background-color: #f4f4f4; }
                            h2 { color: #333333; }
                            table { width: 100%; border-collapse: collapse; margin-top: 20px; }
                            th, td { border: 1px solid #dddddd; text-align: left; padding: 8px; }
                            th { background-color: #4CAF50; color: white; }
                            tr:nth-child(even) { background-color: #f2f2f2; }
                            tr:hover { background-color: #ddd; }
                            a { text-decoration: none; color: #4CAF50; }
                        </style>
                    </head>
                    <body>
                        <h2>Jenkins Idle Jobs Report</h2>
                        <p>Below is a summary of jobs that have been idle for more than the specified threshold of 30 days.</p>
                        <table>
                            <tr>
                                <th>Job Name</th>
                                <th>Days Idle</th>
                            </tr>
                    """

                    report.each { entry ->
    def daysIdle = entry[1] instanceof Number ? entry[1].toDouble().round() : "Never built"
    def jobNameEncoded = URLEncoder.encode(entry[0], "UTF-8").replaceAll("%2F", "/").replaceAll("\\+", "%20")
    reportOutput += """
        <tr>
            <td><a href="${Jenkins.instance.rootUrl}job/${jobNameEncoded}/">${entry[0]}</a></td>
            <td>${daysIdle}</td>
        </tr>
    """
}




                    reportOutput += """
                        </table>
                    </body>
                    </html>
                    """

                    // Save the report as an HTML file in the workspace
                    writeFile file: 'idle_jobs_report.html', text: reportOutput

                    // Archive the report so it is accessible from the Jenkins job dashboard
                    archiveArtifacts artifacts: 'idle_jobs_report.html'

                    // Publish the HTML report
                    publishHTML([allowMissing: false,
                                 alwaysLinkToLastBuild: true,
                                 keepAll: true,
                                 reportDir: '',
                                 reportFiles: 'idle_jobs_report.html',
                                 reportName: 'Idle Jobs Report',
                                 reportTitles: 'Idle Jobs Report'])
                }
            }
        }

        stage('Send Email Notification') {
            steps {
                script {
                    emailext(
                        to: 'janathkumar@xerago.com', // Add multiple recipients here
                        subject: 'Jenkins Idle Jobs Report',
                        body: """
                            <p>Hello,</p>
                            <p>Please find attached the Jenkins Idle Jobs Report.</p>
                            <p>You can also view the report directly <a href="${env.BUILD_URL}Idle%20Jobs%20Report/idle_jobs_report.html">here</a>.</p>
                            <p>Best Regards,<br/>Jenkins</p>
                        """,
                        from: 'jenkinmailer@xerago.com',  // Custom "From" address
                        replyTo: 'janathkumar@xerago.com',
                        attachLog: false,
                        attachmentsPattern: 'idle_jobs_report.html',
                        mimeType: 'text/html'
                    )
                }
            }
        }
    }

    post {
        always {
            script {
                // Clean up workspace
                deleteDir()  // Deletes the entire workspace
            }
        }
    }
}

@NonCPS
def getIdleJobsReport() {
    def idleDaysThreshold = 30
    def now = new Date()
    def report = []

    Jenkins.instanceOrNull?.getAllItems(hudson.model.Job.class)?.each { job ->
        def lastBuild = job.getLastBuild()
        if (lastBuild != null) {
            def daysIdle = (now.time - lastBuild.getTimeInMillis()) / (1000 * 60 * 60 * 24)
            if (daysIdle > idleDaysThreshold) {
                report << [job.fullName, daysIdle]
            }
        } else {
            report << [job.fullName, "Never built"]
        }
    }
    return report
}
