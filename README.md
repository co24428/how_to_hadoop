# how_to_hadoop

# Hadoop  

## 기본 특성  
- 마스터 ( name node ), 슬레이브 ( Datanode )
- 1기가를 한번에 vs 1/n으로 나눠서  
  => 속도면에서 후자가 앞설 수밖에 없다


## 구성하는 법
- 물리적으로 컴퓨터를 나누어서 구성 => 현실적으로 힘듬
- 가상으로 메모리를 나누어서 하둡시스템 구성
- 가분산모드  
  => 윈도우에서는 SSH 안잡아줘도 된다, 조금더 편하다.

## 마스터 슬레이브
- 모든 슬레이브 노드와 연결되고 총괄하는 부분
- 고장날 경우, 세컨더리네임노드로 이어지도록 구성 => 예방책
<hr>

# 스파크

## 기본 특성
- 하둡으로 만족 못한 속도 부분 개선
- RAM에 저장하여 속도를 개선  
  => 끄면 사라진다!  
  => 이를 위해서 하둡을 같이 사용한다.
- **하둡과 스파크**는 같이 움직인다.

<hr>

# Hadoop download in Windows

1. jdk check => 1.8.~~
    - java -version
        - 환경변수 > JAVA_HOME => jdk 설정
        - 환경변수 > Path => 절대경로로 해서 \bin까지 붙여준다.
        - where java로 어디 경로 자바를 쓰는지 확인가능

1. hadoop download
    - https://hadoop.apache.org/releases.html  
      => 3.1.2 > binary > ~~~.tar.gz
    - https://drive.google.com/file/d/1ms_mLM_cwo5XwkiVMuB0nAcCx-XfJx5G/view?usp=sharing  
      => 윈도우에 하둡을 설치하기 위함  
      => 위의 압축 푼 것에 bin 안에 덮어쓰기!
    - 환경변수 잡아주기 ( java와 비슷 )  
      => HADOOP_HOME => hadoop 압축 푼 경로 ( 깔끔하게 C 드라이브로 옮기는게 낫다)
      => Path => \bin && \sbin 두 개!!

1. setting ( etc > hadoop > ...)
    1.  haddop-env.cmd (.sh => for linux)
        - 25 line: JAVA_HOME=%JAVA_HOME%  
          => change => C:\Program" "Files\Java\jdk~~  **or**  
          => change => C:\PROGRA~1\Java\jdk~~ ( dir /x => 압축된 경로 확인 가능 )  
        - 90 line: HADOOP_IDENT_STRING=%USERNAME%
          => change => admin ( prompt > set > 하단에 USERNAME 확인!!! )
        - 추가  
          set HADOOP_PREFIX=C:\bigdata\hadoop-3.1.2  
          set HADOOP_CONF_DIR=%HADOOP_PREFIX%\etc\hadoop  
          set YARN_CONF_DIR=%YARN_CONF_DIR%  
          set PATH=%PATH%;%HADOOP_PREFIX%\bin;%HADOOP_PREFIX%\sbin  
    1. core-site.xml
        - configuration 태그에 추가    
      ```html
      
      ```
        
          '''html
              <configuration> <property>
                  <name>fs.default.name</name>
                  <value>hdfs://0.0.0.0:9000</value>
                </property>
                <property>
                <name>hadoop.tmp.dir</name>
                  <value>/c:/bigdata/hadoop-3.1.2/tmp</value>
                </property>
              </configuration>
          '''
          
        - tmp 폴더 생성
    1. hdfs-site.xml
        - configuration 태그에 추가   
        
      ```html
      
      ```
          '''html
                <configuration>
                  <property>
                    <name>dfs.replication</name>
                    <value>1</value>
                  </property>
                  <property>
                    <name>dfs.permissions</name>
                    <value>false</value>
                  </property>
                  <property>
                    <name>dfs.namenode.name.dir</name>
                    <value>/c:/bigdata/hadoop-3.1.2/namenode</value>
                  </property>
                  <property>
                    <name>dfs.datanode.data.dir</name>
                    <value>/c:/bigdata/hadoop-3.1.2/datanode</value>
                  </property>
                </configuration>
          '''
          
        - namenode / datanode 폴더 생성
        - 상단의 replication => 데이터가 깨질 때를 대비해서 복제하는것  
          => 여기서는 가분산 모드, 1개만 쓴다.
    1. mapred-site.xml
        - configuration 태그에 추가    
        
      ```html
      
      ```
          '''
                <configuration>
                  <property>
                    <name>mapreduce.framework.name</name>
                    <value>yarn</value>
                  </property>
                  <property>
                    <name>mapred.job.tracker</name>
                    <value>0.0.0.0:9001</value>
                  </property>
                </configuration>
          '''
    1. yarn-site.xml
        - configuration 태그에 추가    
          '''        
      ```html
      
      ```
        '''html
                <configuration>
                <!-- Site specific YARN configuration properties -->
                  <property>
                    <name>yarn.nodemanager.aux-services</name>
                    <value>mapreduce_shuffle</value>
                  </property>
                  <property>
                    <name>yarn.nodemanager.aux-services.mapreduce_shuffle.class</name>
                    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
                  </property>
                  <property>
                    <name>yarn.log-aggregation-enable</name>
                    <value>true</value>
                  </property>
                  <property>
                    <name>yarn.nodemanager.pmem-check-enabled</name>
                    <value>false</value>
                  </property>
                  <property>
                    <name>yarn.nodemanager.vmem-check-enabled</name>
                    <value>false</value>
                  </property>
                </configuration>
          '''
    1. workers ( or slaves -> 버전에 따라 다름 )
        - 얘는 원래 나열되어야 함.
        - 여기서는 가분산모드이기 때문에 1개만 있다!!  
          => localhost
                  
      ```html
      
      ```
            '''html
                localhost
                192.168.0.43 datanaode1
            '''  
1. cmd prompt ( **관리자 권한!** )
    1. hdfs namenode -format
        - Storage directory C:\bigdata\hadoop-3.1.2\namenode has been successfully formatted.
    1. start-dfs.cmd
    1. start-yarn.cmd
    1.jps  
            
      ```html
      
      ```
        '''html
            14980 NodeManager
            8132 DataNode
            2024 NameNode
            6968 Jps
            10332 ResourceManager
        '''
        - 5개가 나와야 한다.
    1. localhost:9870/  
      => 제대로 뜨면 성공적!!
    1. stop-yarn.cmd
    1. stop-dfs.cmd

<hr>

# tutirial => after install

<hr>

### 시작하기 전에 하둡 켜야 한다!
- hdfs dfs -[리눅스 명령]
    - hdfs dfs -mkdir /airline
    - hdfs dfs -put ./2008.csv /airline
    - hdfs dfs -ls /airline
        
- hadoop mapreduce example
    - WordCount.jar => 맵리듀스로 워드카운트가 java로 코딩된 파일
    - 1번 파라미터를 사용해서 2번 파라미터에 반환  
      => hadoop jar WordCount.jar /speech/ /output/word_count
    - 체크  
      => hdfs dfs -head /output/word_count/part-r-00000  
      => hdfs dfs -cat /output/word_count/part-r-00000
        
