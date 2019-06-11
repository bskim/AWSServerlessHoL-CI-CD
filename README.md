AWS 서버리스 서비스를 이용한 웹 애플리케이션 구축하기 Hands-on Lab
====

이 실습은 301 레벨로, **Cloud9**을 이용해서 아래의 Lab1을 진행하고, **CodeStar**를 이용해서 Lab2를 진행합니다. 도전과제를 통해서 Lab2의 환경에 Lab1의 개발 코드를 배포하여 CICD 배포 파이프라인을 구축할 수 있습니다.
* Lab1에서는  **Cloud9 IDE**를 이용하여 웹 애플리케이션 구축을 위하여  **프로젝트를 생성**하고,  **코드를 생성**하고, **SAM(Serverless Architecture Model) 템플릿**을 직접 만들어  배포 하는 방법을 학습합니다.
* Lab2에서는 **CodeStar**를 이용하여 DevOps 환경으로 서버리스 서비스를 활용한 CI/CD 프로세스를 구축하여 빌드 단계에서  **단위 테스트**를 하고  **Canary 배포**와  **롤백** 방법을 학습합니다.

## 실습 소개
### 실습 목적
1. **Cloud9을 이용하여 서버리스 애플리케이션을 SAM(Serverless Application Model) 기반으로 개발/배포할 수 있습니다.** 
    - Cloud9 IDE
1. **SAM을 이해하고 서버리스 서비스에 필요한 서비스를 직접 기술 할 수 있습니다.**
    - API Gateway, Lambda, DynamoDB, S3, SNS
1. **서버리스 애플리케이션을 AWS Code 시리즈를 이용하여 배포 프로세스를 제작할 수 있습니다.**
    - CodePipeline, CodeCommit, CodeBuild, CloudFormation(SAM), CodeDeploy
1. X-ray를 이용하여 서버리스 서비스에 대한 모니터링 및 디버깅을 할 수 있습니다.

### 실습 비용
해당 실습은 3시간 기준으로 비용을 예측합니다. 프리티어 기준으로 비용이 발생하지 않는 범위내에서 실습을 진행할 수 있습니다.
마지막 도전과제 수행시 CodePipeline이 증가하면서 비용이 발생할 수 있는 부분이 있으므로 실습 후 리소스를 반드시 삭제해야 합니다.

<sub>
<table class="ImportedHTMLTable">
<colgroup> <col/> <col/> <col/> <col/> <col/> <col/> <col/> </colgroup><tbody><tr><th class="confluenceTh">Service</th><th class="confluenceTh">Sub Service</th><th class="confluenceTh">Unit</th><th class="confluenceTh">Pricing</th><th class="confluenceTh">Use</th><th class="confluenceTh">free tier(12개월)</th><th colspan="1" class="confluenceTh">etc</th></tr><tr><td rowspan="2" class="confluenceTd"><a href="https://aws.amazon.com/ko/cloud9/pricing/" class="external-link" rel="nofollow">Cloud9</a></td><td class="confluenceTd">EC2 - t2.micro</td><td class="confluenceTd">1 h</td><td class="confluenceTd">$0.0116/Hour</td><td class="highlight-green confluenceTd" data-highlight-colour="green">t2.micro 2hour</td><td class="confluenceTd">750hour/month</td><td colspan="1" class="confluenceTd"><br/></td></tr><tr><td class="confluenceTd">EBS - 8GB</td><td class="confluenceTd">1 m</td><td class="confluenceTd">$0.1/GB</td><td class="highlight-green confluenceTd" data-highlight-colour="green">8GB 2hour</td><td class="confluenceTd">30GB/month</td><td colspan="1" class="confluenceTd"><br/></td></tr><tr><td class="confluenceTd"><a href="https://aws.amazon.com/ko/lambda/pricing/" class="external-link" rel="nofollow">Lambda</a></td><td class="confluenceTd">-</td><td class="confluenceTd">128MB</td><td class="confluenceTd"><span style="color: rgb(51,51,51);">$0.000000208/100ms</span></td><td class="highlight-green confluenceTd" data-highlight-colour="green">1,000번 이하</td><td class="confluenceTd">100만번 요청<br/>320만 초 실행</td><td colspan="1" class="confluenceTd">1년후 만료 없음</td></tr><tr><td class="confluenceTd"><a href="https://aws.amazon.com/ko/api-gateway/pricing/" class="external-link" rel="nofollow">API Gateway</a></td><td class="confluenceTd">-</td><td class="confluenceTd"><br/></td><td class="confluenceTd">1백만 API 호출당 $3.50<br/>처음 10TB에 대해 $0.09/GB</td><td class="highlight-green confluenceTd" data-highlight-colour="green">1,000번 이하</td><td class="confluenceTd"><p>호출 100만건<br/>처음 10TB에 대해 $0.09/GB</p></td><td colspan="1" class="confluenceTd"><br/></td></tr><tr><td class="confluenceTd"><a href="https://aws.amazon.com/ko/dynamodb/pricing/" class="external-link" rel="nofollow">DynamoDB</a></td><td class="confluenceTd">-</td><td class="confluenceTd"><br/></td><td class="confluenceTd"><span style="color: rgb(51,51,51);">WCU당 최소 $0.47</span> <br/>RCU당 최소 $0.09<br/>GB당 최소 $0.25</td><td class="highlight-green confluenceTd" data-highlight-colour="green">WCU 5<br/>RCU 5<br/>스토리지 100M</td><td class="confluenceTd"><p>매달 2억건 요청<br/>WCU 25 / RCU 25<br/>인덱싱된 데이터 스토리지 25G</p></td><td colspan="1" class="confluenceTd">1년 후 <br/>지속 제공</td></tr><tr><td colspan="1" class="confluenceTd"><a href="https://aws.amazon.com/ko/sns/pricing/" class="external-link" rel="nofollow">SNS</a></td><td colspan="1" class="confluenceTd">-</td><td colspan="1" class="confluenceTd"><br/></td><td colspan="1" class="confluenceTd">처음 1GB/월 $0.000 GB당<br/>최대 10TB/월 $0.090 GB당</td><td class="highlight-green confluenceTd" colspan="1" data-highlight-colour="green">100M 이하</td><td colspan="1" class="confluenceTd"><span style="color: rgb(51,51,51);">매달 15GB의 데이터 전송</span></td><td colspan="1" class="confluenceTd"><br/></td></tr><tr><td colspan="1" class="confluenceTd"><a href="https://aws.amazon.com/ko/polly/pricing/" class="external-link" rel="nofollow">Polly</a></td><td colspan="1" class="confluenceTd">-</td><td colspan="1" class="confluenceTd"><br/></td><td colspan="1" class="confluenceTd">1백만 문자당 $4</td><td class="highlight-green confluenceTd" colspan="1" data-highlight-colour="green">1,000 문자 이하</td><td colspan="1" class="confluenceTd">매달 문자 500만개</td><td colspan="1" class="confluenceTd"><br/></td></tr><tr><td rowspan="5" class="confluenceTd">CodeStar</td><td class="confluenceTd"><a href="https://aws.amazon.com/ko/codepipeline/pricing/" class="external-link" rel="nofollow">CodePipeline</a></td><td class="confluenceTd"><br/></td><td class="confluenceTd"><span style="color: rgb(51,51,51);">월별 활성 파이프라인*당 $1</span></td><td class="highlight-yellow confluenceTd" data-highlight-colour="yellow">2개 (도전과제 시도시)</td><td class="confluenceTd"><span style="color: rgb(51,51,51);">매월 활성 파이프라인 1개</span></td><td colspan="1" class="confluenceTd"><br/></td></tr><tr><td class="confluenceTd"><a href="https://aws.amazon.com/ko/codecommit/pricing/" class="external-link" rel="nofollow">CodeCommit</a></td><td class="confluenceTd"><br/></td><td class="confluenceTd">최초 5명 초과시 <span style="color: rgb(51,51,51);">월별 $1<br/>GB당 $0.06/월 (10GB/월 초과) <br/>Git 요청당 $0.001 (1000회/월 초과)</span></td><td class="highlight-green confluenceTd" data-highlight-colour="green">1개</td><td class="confluenceTd">최초 5명까지<br/>매달 50GB의 스토리지<br/>매달 10,000건의 Git 요청</td><td colspan="1" class="confluenceTd">1년후 만료 없음</td></tr><tr><td colspan="1" class="confluenceTd"><a href="https://aws.amazon.com/ko/codebuild/pricing/" class="external-link" rel="nofollow">CodeBuild</a></td><td colspan="1" class="confluenceTd"><br/></td><td colspan="1" class="confluenceTd">build.general1.small $0.005/분</td><td class="highlight-green confluenceTd" colspan="1" data-highlight-colour="green">30분</td><td colspan="1" class="confluenceTd"><span style="color: rgb(51,51,51);">매월 100 빌드 분의 build.general1.small</span></td><td colspan="1" class="confluenceTd">1년 후 <br/>지속 제공</td></tr><tr><td class="confluenceTd"><a href="https://aws.amazon.com/ko/codedeploy/pricing/" class="external-link" rel="nofollow">CodeDeploy</a></td><td class="confluenceTd"><br/></td><td class="confluenceTd"><span style="color: rgb(51,51,51);">EC2 또는 Lambda에 배포 무료</span></td><td class="highlight-green confluenceTd" data-highlight-colour="green">30회</td><td class="confluenceTd">-</td><td colspan="1" class="confluenceTd"><br/></td></tr><tr><td colspan="1" class="confluenceTd"><a href="https://aws.amazon.com/ko/cloudformation/pricing/" class="external-link" rel="nofollow">CloudFormation</a></td><td colspan="1" class="confluenceTd"><br/></td><td colspan="1" class="confluenceTd">-</td><td class="highlight-green confluenceTd" colspan="1" data-highlight-colour="green">-</td><td colspan="1" class="confluenceTd">-</td><td colspan="1" class="confluenceTd"><br/></td></tr><tr><td class="confluenceTd"><a href="https://aws.amazon.com/ko/s3/pricing/" class="external-link" rel="nofollow">s3</a></td><td class="confluenceTd">-</td><td class="confluenceTd"><br/></td><td class="confluenceTd">PUT, COPY, POST 또는 LIST 요청 $0.005/1,000건<br/>GET, SELECT 및 기타 모든 요청 $0.0004/1,000건<br/>처음 50TB/월 $0.023/GB</td><td class="highlight-green confluenceTd" data-highlight-colour="green"><p>Put 100번 내외</p><p>GET 2,000번 내외</p></td><td class="confluenceTd">매달 5GB 스토리지<br/>Get 요청 20,000건<br/>Put 요청 2,000건</td><td colspan="1" class="confluenceTd"><br/></td></tr></tbody>
</table>
</sub>



### 실습 종료 후 리소스 삭제
**<font color="red">실습이 종료되고 나면 리소스를 반드시 삭제해야 합니다.</font>**  해당 실습은 CloudFormation 기반으로 진행됩니다. 즉 실습에 사용한 Stack을 삭제하면, 배포되어 있는 리소스도 함께 삭제할 수 있습니다. 반드시 실습에 사용한 리소스가 삭제되었는지 확인 후 실습을 종료합니다. 만약 S3 버킷에 객체가 남아 있을 경우 해당 버킷이 삭제가 되지 않을 수 있으므로, 해당 S3 버킷을 직접 삭제하고 확인 합니다.



## 서버리스 웹 애플리케이션 구축 방법 소개
아래 다이어그램은 Lab1에서 구축할 서버리스 웹 애플리케이션입니다. 서버리스 서비스를 이용하기 때문에, 프로비저닝, 패치, 확장에 대해 고민할 필요가 없으며, 사용한 만큼만 비용을 지불합니다. 또한, 서버리스 서비스는 완전 관리형 서비스이기 때문에, 개발자는 애플리케이션 개발 자체에만 집중할 수 있습니다. 해당 애플리케이션은 텍스트를 전송하면 해당 텍스트를 읽어주는 기능을 서버리스 서비스 기반으로 개발합니다. Lab2에서는 아래의 RESTful API에 대해서 CI/CD 배포 프로세스를 구축하는 방법에 대해서 자세히 살펴봅니다.
![](images/Architecture_diagram.png)

### 애플리케이션의 구성

이 애플리케이션은 다섯 가지 영역으로 구분할 수 있습니다.

1. **정적 웹페이지 구현 - StaticWebBucket**
    1. 이 시나리오는 Amazon S3(Simple Storage Service)에서 호스팅되는 정적 웹 페이지를 기반으로 실행합니다.
1. **새로운 뉴스를 등록 - PostNews**
    1. MP3로 생성할 텍스트 정보는 Amazon API Gateway에 의해 노출된 RESTful API로 수신합니다.
    2. Amazon API Gateway는 MP3 파일 생성 프로세스를 초기화하는 전용 Lambda 함수인 "PostNews"를 설정합니다.
    3. "PostNews" Lambda 함수는 News에 대한 메타 정보를 "NewsTable" DynamoDB 테이블에 저장합니다.
    4. TTS 변환을 비동기적으로 실행하기 위해 Amazon Polly에 StartSpeechSynthesisTask 작업을 예약하고, 작업 ID를 DynamoDB에 추가합니다.
1. **MP3 저장 정보 업데이트 - UpdateNews**
    1. Amazon Polly에서 작업해서 만들어진 MP3 파일은 전용 S3 버킷인 "PollyMp3Bucket"에 저장합니다.
    2. 작업이 완료되면 Topic에 의해서 트리거된 Lambda 함수인 "UpdateNews"를 호출합니다.
    3. "UpdateNews" Lambda 함수는 작업 ID를 확인하고, DynamoDB에 등록된 해당 작업에 대한 상태를 변경합니다. (scheduled → COMPLETE)
1. **등록된 뉴스 정보 검색 - GetNews**
    1. GET 메서드를 이용해서 뉴스에 대한 정보를 검색하는 방법을 제공합니다.
    2. "GetNews" Lambda 함수는 DynamoDB 테이블에서 뉴스 텍스트에 대한 정보와 버킷에 업로드한 MP3 버킷 URL 정보를 제공합니다.
1. **기존 뉴스 삭제 - DeleteNews**
    1. RESTful API로 Delete 메서드를 이용하여 삭제를 요청합니다.
    2. "DeleteNews" Lambda 함수는 DynamoDB에 저장되어 있는 해당 News 항목과 관련되어 생성한 MP3를 삭제합니다.

## 실습

> ⓘ 실습은 Lab1과 Lab2로 구성되어져 있습니다.
> * Lab1에서는 SAM 템플릿을 작성해 나가는 방법과 이해할 수 있습니다.
> * Lab2의 경우 Lab1과 별도로 진행할 수 있도록 구성되어져 있기 때문에 별도로 진행할 수 있습니다.
다만, Lab2는 SAM 템플릿에 대한 이해를 기반으로 빌드 환경을 위한 buildspec을 구성하므로 Lab1 실습 이후에 Lab2를 하는 것을 권장합니다.

### Lab1. Cloud9을 이용한 서버리스 웹 애플리케이션 구축
Lab1은 Cloud9 IDE를 이용하여 서버리스 웹 애플리케이션을 구축하는 방법을 살펴봅니다. 이 실습은 Cloud9이 존재하는 Singapore 리전에서 English 언어로 진행합니다. Cloud9 통합 개발 환경(IDE)은 싱가포르 리전을 사용하지만, 서비스 배포는 다른 리전을 선택할 수 있습니다. 이 실습에서는 Singapore 리전을 그대로 사용하겠습니다.

#### 1. Cloud9 IDE 환경 생성
1. AWS 콘솔 환경에서  Cloud9 서비스로 이동합니다.
![](images/001_Cloud9_Service.png)
2. **Create environment**버튼을 클릭하여 Cloud9 환경을 생성합니다.
![](images/002_Cloud9_Create_environment.png)
3. Cloud9 이름을 **NewsWebApp**이라고 생성합니다. 설명(**News Web Application using Serverless Service**은 옵션이기 때문에 넣지 않아도 됩니다.
![](images/003_Cloud9_Naming.png)
4. Cloud9의 환경설정을 합니다. 개발환경은  **EC2 인스턴스**로 선택하고 인스턴스 타입은  t2.micro  를 선택합니다. 프리티어로 사용할 수 있습니다. Cloud9 IDE를 30분간 사용하지 않으면, 자동으로 EC2 인스턴스가 Stop 되어 비용을 절감할 수 있는 옵션을 기본적으로 제공합니다
![](images/004_Cloud9_Configure.png)
5. Cloud9 최종 점검을 합니다. 모든 검수가 완료되면 **Create environment**버튼을 클릭합니다.
![](images/005_Cloud9_Review.png)
6. Cloud9 IDE가 준비중인 것을 확인할 수 있습니다. 몇 분이 지나면 IDE가 활성화 됩니다.
![](images/006_Cloud9_Watting.png)
7. Cloud9에서 IDE에 대해서 환경 설정을 할 수 있습니다. 화면과 같이 **서비스 배포를 위한 리전을 별도로 지정**할 수 있습니다. 여기서는  *Singapore*리전을 그대로 사용합니다.
![](images/007_Cloud9_AWS_region.png)
8. 개발 환경을 설치하였고, 서버리스 애플리케이션 개발을 할 준비가 완료되었습니다

#### 2. Application 및 "PostNews" Lambda 함수 생성
1. 새로운 애플리케이션 및 람다 함수를 생성합니다. Cloud9 IDE 우측 네비게이션의  **AWS Resources** 탭을 클릭하고,  **Lambda 아이콘** 을 클릭하면 새로운 함수를 생성할 수 있습니다. 이 실습에서는 총 4개의 Lambda 함수를 만들 것이기 때문에, 앞으로 추가할 나머지 3개의 함수도 동일한 방법으로 생성할 예정입니다.
![](images/008_Cloud9_New_Lambda.png)
2. 화면 중앙에 팝업 창이 표시됩니다. 하단의 Application name에는 **WebApp**을 상단의 Function name에는  **PostNews** 를 입력하고,  **Next** 버튼을 클릭합니다. 앞으로 추가할 나머지 3개의 함수도 Application name은 동일하게  **WebApp** 을 사용합니다.
![](images/009_Cloud9_PostNews_Lambda.png) 
3. Runtime은  **Python 2.7**을 선택하고, 하단의 Select blueprint는  **hello-world-python**을 선택하고,  **Next** 버튼을 클릭합니다.
![](images/010_Cloud9_PostNews_Blueprint.png) 
4. PostNews 함수를 호출하는 주체는 **API Gateway**입니다. 따라서, Function trigger는 **API Gateway**를 선택합니다. API 호출에 사용할 리소스 경로는  **/news** 를 입력합니다. API 호출을 위한 보안 메커니즘은  **NONE** 으로 선택하고,  **Next** 버튼을 클릭합니다.
![](images/011_Cloud9_PostNews_Trigger.png) 
5. PostNews 함수를 실행하는 Memory는  **128MB** 로 지정합니다. Role은  **Automatically generate role** 을 선택하고, Next 버튼을 클릭합니다. Role에 들어갈 정책(Policy)는 SAM 템플릿에서 직접 설정할 예정입니다. 
![](images/012_Cloud9_PostNews_Memory.png) 
6. PostNews 함수 구성을 확인합니다. 별도의 이상이 없을 경우,  **Finish**  버튼을 클릭 합니다.
![](images/013_Cloud9_PostNews_Review.png) 
7. 다음과 같이 Application과 Function이 세팅되는 것을 확인할 수 있습니다.
![](images/014_Cloud9_PostNews_Code_origin.png)
    1. 좌측 Environment 탭에  **WebApp**  Application 폴더를 생성합니다. 그리고  **PostNews** 함수에서 사용하는 폴더를 생성합니다.
    2. SAM 템플릿 파일인  **template.yaml** 파일이 생성되는것을 확인할 수 있습니다.
    3. Lambda 함수가 blueprint에 의해서 자동으로 만들어집니다. 해당 코드는 아래에서 제공하는 PostNews 함수 코드로 대체합니다.
    4. 우측 AWS Resources 탭에 Local Functions에 현재 작업중인  **WebApp** 폴더와 Application과  **PostNews** 함수를 생성하는 것을 확인할 수 있습니다.
    5. 자동으로 template을 기반으로 Lambda 서비스에 배포할 수 있습니다. 배포할 경우, 이름은  **Cloud9-** 접두사와 Application 이름인  **WebApp-** 이 접두사로 붙는 것을 확인할 수 있습니다.
8. PostNews 함수는 아래에서 제공하는 **코드로 대체**하고  <font color="red">**Ctrl+S를 눌러서 저장**</font>합니다. 코드에 대한 설명은 주석을 참조하십시오.
![](images/image2018-10-23_9-28-23.png)


|**PostNews**|
---------
|
```python 
# -*- coding: utf-8 -*-
from __future__ import print_function
  
import boto3
import os
import json
import uuid
import datetime
import re
 
def lambda_handler(event, context):
    if "body" in event:
        parmas = json.loads(event['body'])
     
    voice = parmas["voice"]
    originText = parmas["text"]
    timbre = parmas["timbre"]
    pitch = parmas["pitch"]
    updateDate = datetime.datetime.now().strftime("%Y%m%d")
  
    polly = boto3.client('polly')
    removeBrackets = re.sub(r'\([^)]*\)', '', originText)
    repTextBlock = re.sub('[·…]', '<break time="100ms"/>', removeBrackets)
    ssmlBlock = "<speak><amazon:effect vocal-tract-length=\"" + timbre + "\"><prosody pitch=\"" + pitch + "\">" + repTextBlock + "</prosody></amazon:effect></speak>"
    ssmlBlock = ssmlBlock.replace('철수', '<amazon:effect vocal-tract-length="+80%"><prosody pitch="-70%">안녕하세요? 저는 서연 친구 철수에요.</prosody></amazon:effect>')
    ssmlBlock = ssmlBlock.replace('귀신', '<amazon:effect name="whispered"><amazon:effect vocal-tract-length="-30%"><prosody volume="loud">나 꿍꼬또, 기싱꿍꼬또</prosody></amazon:effect></amazon:effect>')
    print (ssmlBlock)
      
    response = polly.start_speech_synthesis_task(
        OutputFormat= 'mp3',
        OutputS3BucketName= os.environ['BUCKET_NAME'],
        # OutputS3KeyPrefix='polly/',
        SampleRate='22050',
        SnsTopicArn=os.environ['SNS_TOPIC'],
        Text=ssmlBlock,
        TextType='ssml',
        VoiceId=voice
    )
    print (response)
    data = response['SynthesisTask']
      
    # Creating new record in DynamoDB table
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['DB_TABLE_NAME'])
    table.put_item(
        Item = {
            'id' : data['TaskId'],
            'updateDate': data['CreationTime'].strftime("%Y-%m-%d %H:%M:%S"),
            'voice' : voice,
            'originText': originText,
            'pollyStatus' : data['TaskStatus'],
            'timbre': timbre,
            'pitch': pitch,
            'mp3Url': data['OutputUri'],
            'RequestCharacters': data['RequestCharacters']
        }
    )
     
    result = {
        'statusCode': 200,
        'body': json.dumps({'recordId': data['TaskId']}),
        'headers': {
            'Content-Type': 'application/json',
            'Access-Control-Allow-Origin': '*'
        }
    }
      
    return result

```|