# HOW_TO_HADOOP_IN_MACOS

1. jdk check
  - Mac의 Java jdk 경로  
    /Library/Java/JavaVirtualMachines/
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
    
2. hadoop install
  - <a> https://hadoop.apache.org/releases.html</a>  
      ~~~
        export HADOOP_HOME=/Users/kimyihwan/bigdata/hadoop-3.2.1
        export PATH=${PATH}:$HADOOP_HOME/bin
        export PATH=${PATH}:$HADOOP_HOME/sbin  
      ~~~
  - 위에서처럼 환경변수 설정
