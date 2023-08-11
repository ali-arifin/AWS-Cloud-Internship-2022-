# Task No. 7
## :memo: Installed softwares on one Windows EC2, created AMI from it and used it to launch multiple EC2s
:heavy_check_mark: **Created an AMI from SourceInstance which has Zoom & Chrome installed. Two new instances are launched from this AMI:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/a4c6c446-ec9a-4808-8210-1ae43f73c5cb)




#  
:heavy_check_mark: **Repeated the task by CloudFormation:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/9f91cf39-788e-4913-9632-e51d55a63011)




#
:heavy_check_mark: **CloudFormation YAML code:**
```yaml
Resources:
  SourceInstance:
    Type: AWS::EC2::LaunchTemplate
    Properties:
      LaunchTemplateName: MyTemplate
      LaunchTemplateData:
        InstanceType: t2.micro
        ImageId: ami-07a53499a088e4a8c
        KeyName: ali-key-pair
        UserData:
          Fn::Base64: 
            !Sub |
              <powershell>

              $Path = $env:TEMP; $Installer = "chrome_installer.exe"; Invoke-WebRequest "http://dl.google.com/chrome/install/375.126/chrome_installer.exe" -OutFile $Path\$Installer; Start-Process -FilePath $Path\$Installer -Args "/silent /install" -Verb RunAs -Wait; Remove-Item $Path\$Installer
            
              $workdir = "c:\installer\"
              If (Test-Path -Path $workdir -PathType Container)
              { Write-Host "$workdir already exists" -ForegroundColor Red}
              ELSE
              { New-Item -Path $workdir  -ItemType directory }
              $source = "https://download.mozilla.org/?product=firefox-latest&os=win64&lang=en-US"
              $destination = "$workdir\firefox.exe"
              if (Get-Command 'Invoke-Webrequest')
              {
                   Invoke-WebRequest $source -OutFile $destination
              }
              else
              {
                   $WebClient = New-Object System.Net.WebClient
                   $webclient.DownloadFile($source, $destination)
              }
              Start-Process -FilePath "$workdir\firefox.exe" -ArgumentList "/S"
              Start-Sleep -s 35
              rm -Force $workdir/firefox*

              </powershell>

  Instance1WithSoftware:
    Type: AWS::EC2::Instance
    Properties:
      LaunchTemplate: 
        LaunchTemplateName: MyTemplate
        Version: !GetAtt SourceInstance.DefaultVersionNumber
        
  Instance2WithSoftware:
    Type: AWS::EC2::Instance
    Properties:
      LaunchTemplate: 
        LaunchTemplateName: MyTemplate
        Version: !GetAtt SourceInstance.DefaultVersionNumber
```

:pushpin: **The stack creates an EC2 Launch Template with PowerShell commands for software installation in user data. Then two instances are launched following the template.**
