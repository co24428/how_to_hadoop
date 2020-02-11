# HOW_TO_HADOOP_IN_MACOS

1. **jdk check**
    - Mac의 Java jdk 경로  
      /Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk  
    - Mac의 jdk bin 경로  
      /Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk/Contents/Home  

    - 환경변수 설정
        1. cd /Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk/Contents/Home
        1. vi ~/.bash_profile  
          => 처음이면 New File  
          => i, 입력모드 진입
        1. 입력  
            ~~~
              export JAVA_HOME=Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk/Contents/Home
              export PATH=${PATH}:$JAVA_HOME/bin
            ~~~
        1. esc > :wq (save & exit)
        1. 환경변수 확인  
          => source ~/.bash_profile
        1. 환경변수 체크  
          => echo $PATH

1. **hadoop install**
    - <a> https://hadoop.apache.org/releases.html</a>  
        ~~~
          export HADOOP_HOME=/Users/[user]/bigdata/hadoop-3.2.1
          export PATH=${PATH}:$HADOOP_HOME/bin
          export PATH=${PATH}:$HADOOP_HOME/sbin  
        ~~~
    - 위에서처럼 환경변수 설정

1. **setting ( etc > hadoop > ...)**

    1. hadoop-env.sh  
        - configuration 태그에 추가
            ~~~
              export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_221.jdk/Contents/Home

              export HADOOP_HEAPSIZE=2000

              export HADOOP_OPTS="-Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk"
            ~~~
        - 여기까지만 진행해도 하둡이 잡힌다.
        - hdfs를 bash에 치면 잘 동작한다.(도움말이 주욱 나온다.)
          
    1. core-site.xml  
        - configuration 태그에 추가
            ~~~
              <configuration> 
                <property>
                  <name>fs.default.name</name>
                  <value>hdfs://0.0.0.0:9000</value>
                </property>
                <property>
                <name>hadoop.tmp.dir</name>
                  <value>/Users/[user]/bigdata/hadoop-3.2.1/tmp</value>
                </property>
              </configuration>
            ~~~
            
    1. hdfs-site.xml  
        - configuration 태그에 추가
            ~~~
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
                  <value>/Users/[user]/bigdata/hadoop-3.2.1/namenode</value>
                </property>
                <property>
                  <name>dfs.datanode.data.dir</name>
                  <value>/Users/[user]/bigdata/hadoop-3.2.1/datanode</value>
                </property>
              </configuration>
            ~~~
            
    1. mapred-site.xml  
        - configuration 태그에 추가
            ~~~
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
            ~~~
            
    1. yarn-site.xml  
        - configuration 태그에 추가
            ~~~
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
            ~~~
    - 윈도우 설정부분에서 hadoop-env만 MAC에 맞게 잡아주었다.
    - 안해줘도 되는 설정이 있을 것이다..
1. ssh key setting
    - references
        - <a>https://moonlighting.tistory.com/150</a>
    - ssh localhost
        - localhost에 접속하는데 비밀번호가 걸리지 않아야 된다.
        ~~~
          The default interactive shell is now zsh.
          To update your account to use zsh, please run `chsh -s /bin/zsh`.
          For more details, please visit https://support.apple.com/kb/HT208050.
        ~~~
        - 시키는대로 'chsh -s /bin/zsh' 실행해준다. 
    - ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
        - 요상한 그림을 띄워준다.
        ~~~
          +---[DSA 1024]----+
          |  ...o.   .+=o   |
          |   o.oE. ..+o    |
          |  o =..o.o++     |
          | . = oo  +* .    |
          |  o o.o S=+.     |
          |   . + ++.o.     |
          |    o . .+..     |
          |        .oo..    |
          |        .**o.    |
          +----[SHA256]-----+
        ~~~
        - 성공!
    - cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
        - 생성한 ssh 키를 authorized_keys로 잡아준다.
    
    
1. running hadoop
    1. hdfs namenode -format
    1. start-all.sh
    1. jps => 5개가 나와야 한다.
        ~~~
          14980 NodeManager
          8132 DataNode
          2024 NameNode
          6968 Jps
          10332 ResourceManager
        ~~~
    1. localhost:9870 접속 확인
    1. stop-all.sh
