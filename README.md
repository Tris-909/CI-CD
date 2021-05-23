# CI-CD
Learning GitLab CI-CD 

# Create a basic pipeline with 2 phase :

```
stages:
  - build
  - test

Build The Car:
  stage: build
  script: 
    - mkdir build 
    - cd build 
    - touch car.txt
    - echo "Adding randomText" >> car.txt
  artifacts: 
    paths: 
      - build/
      
 Testing:
   stage: test
   script: 
     - test -f build/car.txt
     - cd build 
     - grep "Adding randomText" car.txt
```

- A Job contain of : stage ( if you have more than 1 stage ), script ( script to run ), artifacts ( temporary places to save data from this job and pass to next job if you have next job )
- stages in .gitlab-ci.yaml file is where you decide which job will be executed first and which job will be the second

# A job in CI-CD 

```
DeployWebsite: 
  stage: deploy
  environment:  
    name: production
    url: http://$PRODUCTION_DOMAIN
  when: manual 
  only:
    - master
  script: 
    - echo $CI_COMMIT_SHORT_SHA
    - npm install 
    - npm install --global surge
    - npm install -g gatsby-cli
    - npm run build 
    - sed -i "s/%%VERSION%%/$CI_COMMIT_SHORT_SHA/" ./public/index.html
    - surge --project ./public --domain http://$PRODUCTION_DOMAIN
```

- A job must have 'script'. Other is optional. 
