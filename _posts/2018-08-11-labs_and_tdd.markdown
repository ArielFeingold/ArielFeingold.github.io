---
layout: post
title:      "Labs and TDD"
date:       2018-08-12 02:11:59 +0000
permalink:  labs_and_tdd
---


A big part of my web development course involved passing labs. labs are a good example for Test Driven Development (or TDD for short). TTD is a software development process that implements a short development cycle, requirements are translated to test cases that define the desired result. Designed as an offshoot of extreme programming, TDD follows the agile method of building software in iterations and involves clean, simple designs and code.
 
The work flow of TDD is circular, is starts with writing a test that defines a function (or improvement) that we want to add. The test should be briefly and easily expressed.  In the second step we run the tests and see if they fail- this way we also test the test code itself and see that it fails for the expected reason. The next step is to write some code that causes the test to pass. The code at this stage doesn’t have to be perfect or elegant. It only needs to be sufficient to pass the test. Once the tests pass we refracture the code, improve it and run tests again.

Labs are a great introduction to TDD. You learn not only how to write code but also how to write tests for when you create your own app. 

When coming to building your test it is important to keep a good test Structure. Good layout improves the readability of your tests and smooths the execution. 

A common structure for test is to divide it to 4 sections:


1. Setup- make sure the system or the unit are in the right state to run the test (before assertions).

2. Execution- trigger the target behavior and capture all the output that it returns (expect assumptions).

3.	Validation- Ensure the results of the test are correct. These results may include explicit outputs or state changes during the test run.

4.	Cleanup- restore everything state to the pre-test state. This permits another test to run consecutively after the test completes. 

