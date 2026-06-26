# Desafio DIO - AWS CloudFormation Automatizado

Este projeto faz parte do desafio da **Digital Innovation One (DIO)** sobre infraestrutura como código com **AWS CloudFormation**.  
O objetivo é implementar uma stack automatizada e documentar o processo.

---

## Objetivo do Projeto
- Criar uma stack automatizada com CloudFormation.  
- Provisionar EC2, Security Group e S3.  

---

## Arquitetura da Stack

```text
[AWS CloudFormation Template]
        ↓
[EC2 Instance] + [Security Group]
        ↓
[S3 Bucket]
```

---

## template.yaml
```text
AWSTemplateFormatVersion: '2010-09-09'
Description: Stack automatizada com EC2, SG e S3

Resources:
  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0   # Ajustar conforme região
      SecurityGroups:
        - !Ref MySecurityGroup

  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Permitir acesso SSH
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 22
          ToPort: 22
          CidrIp: 0.0.0.0/0

  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: meu-bucket-automatizado-cloudformation

```

## Passo a passo com AWS CLI

1. Validar template: **aws cloudformation validate-template --template-body file://template.yaml**

2. Criar stack: **aws cloudformation create-stack --stack-name MinhaStackAutomatizada --template-body file://template.yaml**

3. Acompanhar processo: **aws cloudformation describe-stacks --stack-name MinhaStackAutomatizada**

4. Atualizar stack: **aws cloudformation update-stack --stack-name MinhaStackAutomatizada --template-body file://template.yaml**

5. Deletar stack: **aws cloudformation delete-stack --stack-name MinhaStackAutomatizada**

## Saída: describe-stacks

```text
{
    "Stacks": [
        {
            "StackName": "MinhaStackAutomatizada",
            "CreationTime": "2026-06-25T22:40:00.000Z",
            "StackStatus": "CREATE_COMPLETE",
            "StackId": "arn:aws:cloudformation:us-east-1:123456789012:stack/MinhaStackAutomatizada/abcd1234-efgh-5678-ijkl-9012mnop3456"
        }
    ]
}
```
