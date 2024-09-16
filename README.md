name: Maven CI
 
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main
 
jobs:
  build:
    runs-on: ubuntu-latest
 
    steps:
    - name: Checkout code
      uses: actions/checkout@v3
 
    - name: Set up JDK 11
      uses: actions/setup-java@v3
      with:
        java-version: '11'
        distribution: 'adopt'
 
    - name: Cache Maven packages
      uses: actions/cache@v3
      with:
        path: ~/.m2/repository
        key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}
        restore-keys: |
          ${{ runner.os }}-maven-
 
    - name: Build with Maven
      run: mvn clean install
 
    - name: Run tests
      run: mvn test
