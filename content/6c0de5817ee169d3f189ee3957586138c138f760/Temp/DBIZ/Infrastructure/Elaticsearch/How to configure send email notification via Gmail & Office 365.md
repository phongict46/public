**Describe:**
With the email alert feature, you can easily monitor the system's activities and come up with timely solutions, increasing the proactivity in improving the user experience with the enterprise system you provide.
![[Pasted image 20250530110424.png]]

**I.** Create configure file (example: logstash.conf) in patch /et/logstash/conf.d/
```bash
input {
  file {
    type => "json"
    codec => "json"
    path => "/var/log/kibana/kibana.log"
    start_position => beginning
  }
}

filter {
  grok {
    match => {
      "message" => "Server log: - AlertName: %{DATA:alertName}"
  }

  }
}


output {
  if[log][level] == "WARN" {

  email {
    to => "phongict46@gmail.com"  # Replace with your Gmail address
    from => "Kibana_System"  # Replace with a Gmail address you control
    subject => "Kibana WARN Log Alert"
    body => "Log Message:\n\nRaw Message:%{message}"
    address => "smtp.gmail.com"
    # via => "smtp"
    port => 587
    username => "phongict46@gmail.com"  # Replace with your Gmail address
    password => "mzgf ebvw xdtv ogfs"  # Replace with your Gmail password or app password
    use_tls => true
  }

  email {
    to => "phongpt@dbiz.com"  # Replace with your Gmail address
    from => "Kibana_System"  # Replace with a Gmail address you control
    subject => "Kibana WARN Log Alert"
    body => "Log Message:\n\nRaw Message:%{message}"
    address => "smtp.office365.com"
    # via => "smtp"
    port => 587
    username => "phongpt@dbiz.com"  # Replace with your Gmail address
    password => "Ptp@#2025"  # Replace with your Gmail password or app password
    use_tls => true
    authentication => "login"
    debug => true
  }


  stdout {
      codec => line {
        format => "%{message}"
      }
    }
  }
}
```
![[Pasted image 20250530093852.png]]

**II.** Run test logstash with command below. If everything correct you will receive email send to Gmail and Office 365 mail already setup in the paste.
```bash
/usr/share/logstash/bin/logstash -f /etc/logstash/conf.d/logstash.conf
```

Information log show in terminal vm server running logstash service.
![[Pasted image 20250530100025.png]]
**III.** Check email notifications form Logstash/Kibana.
![[Pasted image 20250530094921.png]]
![[Pasted image 20250530095024.png]]

You can access the link in the content log file to go to the Kibana website to display all information about the logs. In the images below, you can see the warning notification that automatically pops up when the CPU exceeds 80% according to the threshold rule settings.
![[Pasted image 20250530110955.png]]

If resole accident you can see notice **Recovered** in Alerts of Kibana website.
![[Pasted image 20250530095429.png]]

Choose Metadata tab for show all information logs.
![[Pasted image 20250530095530.png]]
The end.