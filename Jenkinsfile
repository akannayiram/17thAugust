#!/usr/bin/env/ groovy

import hudson.model.*
import hudson.EnvVars
import java.net.URL

node{
/* ************
  o Wrapping each stage in try-catch-finally
  o Adding post-build in finally after checking currentBuild.currentResult
  by Alagu 2nd Sept 2019
*************** */
// Git checkout
	stage('Git checkout'){
//			currentBuild.currentResult = 'SUCCESS'
		try{
			git 'https://github.com/edureka-git/DevOpsClassCodes'
		} catch (Exception err) {
			currentBuild.currentResult = 'FAILURE'
			throw err
		} finally {
			if (currentBuild.currentResult == 'SUCCESS') {
				echo "Git checkout: currentBuild.currentResult: ${currentBuild.currentResult}"
			}
		}
    }
// Compile
	stage('Compile'){
		try{
			withMaven(maven:'mymaven'){
				sh 'mvn compile'
			}
		} catch (Exception err) {
			currentBuild.currentResult = 'FAILURE'
			throw err
		} finally {
			if (currentBuild.currentResult == 'SUCCESS') {
				echo "Compile: currentBuild.currentResult: ${currentBuild.currentResult}"
			}
		}
	}
// Code Review
	stage('Code Review'){
		try{
			withMaven(maven:'mymaven'){
				sh 'mvn -P metric pmd:pmd'
			}
		} catch (Exception err) {
			currentBuild.currentResult = 'FAILURE'
			throw err
		} finally {
			if (currentBuild.currentResult == 'SUCCESS') {
				echo "Code Review: currentBuild.currentResult: ${currentBuild.currentResult}"
				def pmd = scanForIssues tool: [$class: 'Pmd'], pattern: '**/target/pmd.xml'
				publishIssues issues:[pmd]
				}
		}
	}
// Test
	stage('Test'){
		try{
			withMaven(maven:'mymaven'){
				sh 'mvn test'
			}
		} catch (Exception err) {
			currentBuild.currentResult = 'FAILURE'
			throw err
		} finally {
			if (currentBuild.currentResult == 'SUCCESS') {
				echo "Test: currentBuild.currentResult: ${currentBuild.currentResult}"
				junit 'target/surefire-reports/*.xml'
			}
		}
	}
// Coverage Check
	stage('Coverage Check'){
		try{
			withMaven(maven:'mymaven'){
//				sh 'mvn cobertura:cobertura -Dfile.encoding=iso-8859-1  -Dcobertura.report.format=xml'
				sh 'mvn cobertura:cobertura'
			}
		} catch (Exception err) {
			currentBuild.currentResult = 'FAILURE'
			throw err
		} finally {
			if (currentBuild.currentResult == 'SUCCESS') {
				echo "Coverage Check: currentBuild.currentResult: ${currentBuild.currentResult}"
			}
		}
	}	
// Package
	stage('Package'){
		try{
			withMaven(maven:'mymaven'){
				sh 'mvn package'	
			}
		} catch (Exception err) {
			currentBuild.currentResult = 'FAILURE'
			throw err
		} finally {
			if (currentBuild.currentResult == 'SUCCESS') {
				echo "Package: currentBuild.currentResult: ${currentBuild.currentResult}"
			}
		}
	}
}
