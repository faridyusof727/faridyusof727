# **Using Singleton In Go**

When I was starting to learn Go, I was passing around the instance to access the property of that instance. This is how I do it:

```go
e := echo.New()
db := config.Db{}.Init()

tools.Migrate(db)
tools.InitRoute(e, db)
```

There's a debate on Singleton usage where most people say that it will introduce difficulties while doing testing.

> ***In summary, over time people have found singletons to be less than optimal, and IMHO especially if you are trying to do any test-driven development: on many levels, they are pretty much as bad as global variables. Ref: [stackoverflow.com/a/1823320/14347896](https://stackoverflow.com/a/1823320/14347896)***
>

There are also problems around threads and you will need to properly lock and unlock for any references that are calling the instance.

Let's look at what it looks like when we implement Singleton in Go. And I want to remind you that once you implement this pattern, it requires a very significant effort to refactor when this logic is inevitably wrong.

```go
var lock = &sync.Mutex{}
var firebaseAuthClient *auth.Client

func GetFirebaseInstance() (*auth.Client, error) {
    if firebaseAuthClient == nil {
        lock.Lock() // IMPORTANT: We need to lock here to ensure no one calls the same instance multiple times.
        defer lock.Unlock()
        if firebaseAuthClient == nil {
            opt := option.WithCredentialsFile(os.Getenv("FIREBASE_ADMIN_SDK"))

            app, err := firebase.NewApp(context.Background(), nil, opt)
            if err != nil {
                return nil, err
            }

            client, err := app.Auth(context.Background())
            if err != nil {
                return nil, err
            }

            firebaseAuthClient = client
        }
    }

    return firebaseAuthClient, nil
}
```
