## GithubIssue

`GithubIssue` provides a simple way to mark test cases with open issue.
`@GitHubIssue(issueNumber = <your issue number>, fixed = boolean)` is used to mark test cases with open issue.
If a test case has this annotation and `fixed = false` it must fail. If it doesn't fail it will be marked as failed.
Any test case with this annotation and `fixed = true` behaves like a normal unit test.
We use JUnit 5 annotation composition feature(https://junit.org/junit5/docs/current/user-guide/#writing-tests-meta-annotations), so you only need to add this annotation to your test method.

## Example
```java
@GitHubIssue(issueNumber = 4218, fixed = false)
void testSniperDoesNotPrintTheDeletedAnnotation() {
	Consumer<CtType<?>> deleteAnnotation = type -> {
		type.getAnnotations().forEach(CtAnnotation::delete);
	};
	BiConsumer<CtType<?>, String> assertDoesNotContainAnnotation = (type, result) ->
			assertThat(result, not(containsString("@abc.def.xyz")));
	testSniper("sniperPrinter.DeleteAnnotation", deleteAnnotation, assertDoesNotContainAnnotation);
}
```
Internal `@GitHubIssue` combines `@Test` and `@ExtendWith` annotation with an extension enforcing the specified behavior.

## Original Idea

The original idea and implementation is from Spoon(https://github.com/INRIA/spoon) and discussed in this issue https://github.com/INRIA/spoon/issues/3001.
The code is copied with permission from Spoon.
