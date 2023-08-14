# Task No. 8
## :memo: Created an Amazon WorkSpace
:heavy_check_mark: **Created a Windows WorkSpace:**

![image](https://github.com/ali-arifin/AWS-Cloud-Internship-2022-/assets/103297661/d83c6e9a-8f40-4b52-8715-8653eb65456b)



#
:heavy_check_mark: **CloudFormation YAML code:**
```yaml
Resources:   
  MyWorkspace:
    Type: AWS::WorkSpaces::Workspace
    Properties: 
      BundleId: wsb-8vbljg4r6
      DirectoryId: d-9067bde164
      UserName: Administrator
      WorkspaceProperties: 
        ComputeTypeName: STANDARD
```
