# leanGit
git命令练习

app:
  id: LEAP.6c58776423d336cb794f7ffd3d602177
#env: DEV
#env: SIT
env: POC
apollo:
  bootstrap:
    eagerLoad:
      enabled: true
  cluster: default
  # 该地址对应SIT环境
  #  meta: http://168.63.117.42:8080,http://168.63.117.43:8080,http://168.63.117.44:8080
  # 该地址对应DEV环境
  #  meta: http://168.61.11.7:8085
  # 该地址对应POC环境
  meta: http://168.61.11.191:8080
  cacheDir: cache





spring:
  mvc:
    view:
      suffix: .html
  aop: #aop代理
    proxyTargetClass: true
  application:
    name: gateway
  cloud:
    #网关配置
    gateway:
      discovery:
        locator:
          #开启通过serviceId转发到具体服务
          enabled: true
          #开启识别小写的serviceId
          lowerCaseServiceId: true
      routes:

        - id: wss
          uri: http://localhost:9080/socket.io
          predicates:
              #拦截的路由，当在地址栏输入该地址时会被转发到http://localhost:9080/socket.io
            - Path=/leap/socket.io/**
          filters:
            - StripPrefix=1

        - id: gateway
          uri: http://localhost:8080/index.html
          predicates:
              #拦截的路由，当在地址栏输入该地址时会被转发到http://localhost:8080/index.html
            - Path=/leap

        - id: gateway
          uri: http://localhost:8080
          predicates:
              #拦截的路由，当在地址栏输入该地址时会被转发到http://localhost:8080/static/**
            - Path=/leap/static/**
          filters:
            - StripPrefix=1

        - id: XBRL
          uri: lb://XBRL-SERVICE
          predicates:
            #拦截的路由，当在地址栏输入该地址时会被转发到lb://XBRL-SERVICE/xbrl/**
            - Path=/leap/xbrl/**
          filters:
            - StripPrefix=1

        - id: DXTA
          uri: lb://DXTA-SERVICE
          predicates:
            #拦截的路由，当在地址栏输入该地址时会被转发到lb://DXTA-SERVICE/dxta/**
            - Path=/leap/dxta-api/dxta/**
          filters:
            - StripPrefix=2

        - id: BDS
          uri: lb://BDS-SERVICE
          predicates:
            #断言的路由，当在地址栏输入该地址时会被转发到lb://BDS-SERVICE/bds/**
            - Path=/leap/bds/bds/**
          filters:
            - StripPrefix=2

        - id: SYSM
          uri: lb://PLATFORM-SERVICE
          predicates:
            #拦截的路由，当在地址栏输入该地址时会被转发到lb://PLATFORM-SERVICE/platform/**
            - Path=/leap/platform/platform/**
          filters:
            #排除/leap/platform前缀
            - StripPrefix=2

        - id: OPSCTL
          uri: lb://OPSCTL-SERVICE
          predicates:
            #断言的路由，当在地址栏输入该地址时会被转发到lb://OPSCTL-SERVICE/opsctl/**
            - Path=/leap/opsctl/**
          filters:
            - StripPrefix=1

        - id: MSFA
          uri: lb://MSFA-SERVICE
          predicates:
            #断言的路由，当在地址栏输入该地址时会被转发到lb://MSFA-SERVICE/msfa/**
            - Path=/leap/msfa/**
          filters:
            - StripPrefix=1

        - id: AMDC
          uri: lb://INDEX-SERVICE
          predicates:
            #断言的路由，当在地址栏输入该地址时会被转发到 lb://INDEX-SERVICE/index/**
            - Path=/leap/index/index/**
          filters:
            - StripPrefix=2

        - id: PRICE
          uri: lb://PRICE-SERVICE
          predicates:
            #断言的路由，当在地址栏输入该地址时会被转发到 lb://PRICE-SERVICE/price/**
            - Path=/leap/price/price/**
          filters:
            - StripPrefix=2

        - id: ZJGL
          uri: lb://ZIGL-SERVICE
          predicates:
              #断言的路由，当在地址栏输入该地址时会被转发到 lb://ZJGL-SERVICE/zjgl/**
            - Path=/leap/zjgl/zjgl/**
          filters:
            - StripPrefix=2

        - id: ZLGL
          uri: lb://ZLGL-SERVICE
          predicates:
              #断言的路由，当在地址栏输入该地址时会被转发到 lb://ZLGL-SERVICE/zlgl/**
            - Path=/leap/zlgl/zlgl/**
          filters:
            - StripPrefix=2

#解决504和hystrix time out
ribbon:
  ReadTimeout: 60000
  ConnectTimeout: 60000
  MaxAutoRetries: 0
  MaxAutoRetriesNextServer: 1
hystrix:
  command:
    default:
      execution:
        timeout:
          enabled: true
        isolation:
          thread:
            timeoutInMilliseconds: 60000


