#!/usr/bin/env/ groovy

import hudson.model.*
import hudson.EnvVars
import java.net.URL

node{
    stage('Git Checkout'){
        git 'https://github.com/npsoni88/DevOpsClassCodes.git'
    }
    stage('compile'){
        withMaven(maven:'mymaven'){
            sh 'mvn compile'
        }
    }
    stage('Package'){
        withMaven(maven:'mymaven'){
            sh 'mvn package'
        }
    }
}
