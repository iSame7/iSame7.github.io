---
layout: post
title: "Performance Test in Xcode"
date: 2015-08-27 02:48:33 +0200
comments: true
categories: 
---

Yesterday i recieved a JIRA issue from QA that the iOS application is very slow apparently the issue was in application services(API) being very slow to perform any operation.
I started to investigate in the issue and there was a use case that the user select a file then there was a 2 API that should be called to process this file to be previewed in PDF previewer, So i started to think about a way to calculate the time the process it takes starting from selecting the file untile it's previewed. i eneded up with profiling and therfore i did some research on how to do a profiling in Xcode and i found that Xcode has an addition to unit testing it provide the ability to measure the performance of a piece of code. This allow developers to gain insight into the specific timing infotmation of the code that being tested.

## Performance Testing

With performance testing you can know the average time of execution for a piece of code. Also you can define a baseline exection time. This means that if the code that's being testd deviates from that baseline, the test fails. Xcode will repeatedly execute the code 10 times and measure the average execution time. To measure the measure the performance of a piece of code, use the "measureBlock:" API.
This is one of the test cases i prepared:

{% codeblock lang:swift %}
- (void)testPerformanceExample {
    // This is an example of a performance test case.
    [self measureBlock:^{
        // Put the code you want to measure the time of here.
        XCTestExpectation *completionExpectation = [self expectationWithDescription:@"Long method"];
        [self.vcToTest doConvertFile:@"" withBlock:^(NSString *result) {

            XCTAssertEqualObjects(@"success", result, @"Result was not correct!");
            [completionExpectation fulfill];
            
        }];

        [self waitForExpectationsWithTimeout:100.0 handler:nil];
    }];
}
{% endcodeblock %}

But what is XCTestExpectation in this block of code, Right this is a class that enable you to test Asyncronous code. The flow to test Asynchronous code is as follows, define an expectaion, wait for the expectation to be fulfilled, and fulfill the expectation when the asyncronous code finsish executing.

Then i started to run the above test in Xcode and this was the result:
{% img left images/performance_test_result.png #2 %}

Now you can edit the baseline time of execution for the performance test.