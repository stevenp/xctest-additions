xctest-additions
=============

XCTAsyncTestCase
---------------
XCTest-capable drop-in replacements for [GHUnit](https://github.com/gabriel/gh-unit/)'s class for writing asynchronous tests, `GHAsyncUnitTestCase`.

Drag the files `XCTAsyncTestCase.h` and `XCTAsyncTestCase.m` into your project and add them to your iOS tests target.


Example test on blocks usage, TestAsync.m:
```
@interface TestAsync : XCTAsyncTestCase

@end

@implementation TestAsync
- (void)testBlockSample
{
    [self prepare];
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^(){
        sleep(1.0);
        [self notify:kXCTUnitWaitStatusSuccess];
    });
    // Will wait for 2 seconds before expecting the test to have status success
    // Potential statuses are:
    //    kXCTUnitWaitStatusUnknown,    initial status
    //    kXCTUnitWaitStatusSuccess,    indicates a successful callback
    //    kXCTUnitWaitStatusFailure,    indicates a failed callback, e.g login operation failed
    //    kXCTUnitWaitStatusCancelled,  indicates the operation was cancelled
    [self waitForStatus:kXCTUnitWaitStatusSuccess timeout:2.0];
}
```
