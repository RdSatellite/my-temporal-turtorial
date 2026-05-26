# Example code

Download the official example code:

```bash
git clone https://hithub.com/temporalio/money-transfer-project-template-go
```

then go into it.

## Workflow

Let's go and see workflow in temporal.

A Temporal Application is a set of Temporal **Workflow Executions**, which are reliable, durable function executions.

These Workflow Executions orchestrate the execution of **Activities**, which execute a single, well-defined action, such as calling another service, transcoding a media file, or sending an email message.

Use **Workflow Definiton** to define the Workflow Execution's constraints. A WOrkflow Definition in Go is a regular Go function that accepts a Workflow Context and some input values.

In the sample project, they defined a workflow to describe how money transfer:

```go
func MoneyTransfer(ctx workflow.Context, input PaymentDetails) (string, error) {
    // Retry Policy specifies how to automatically handle retries if an Activity fails.
    retrypolicy := &temporal.RetryPolicy{
        InitialInterval: time.Second,
        BackoffCoefficient: 2.0,
        MaximumInterval: 100 * time.Second,
        MaximumAttempts: 500, // 0 is unlimited retries
        NonRetryableErrorTypes: []string{"InvalidAccountError", "InsufficientFundsError"},
    }

    options := workflow.ActivityOptions{
        // Timeout options specify when to automatically timeout Activity functions.
        StartToCloseTimeout: time.Minute,
        // Optionally provide a customized RetryPolicy.
        // Temporal retries failed Activities by default.
        RetryPolicy: retrypolicy,
    }
}
```
