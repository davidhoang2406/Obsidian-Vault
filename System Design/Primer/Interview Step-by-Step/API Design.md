### API Design

After defining our requirements, it’s time to move on to the API design stage. Again, this stage should only take a few minutes.

A lot of people really struggle at this part simply because they overcomplicate it. Ultimately, all this stage is is just turning your functional requirements into API endpoints. To keep it simple, there should be one endpoint per functional requirement.

In this section, interviewers are looking for three things:

1. Readable paths: You should pick easily understandable names. For an endpoint that deals with Tweets, /tweet makes a lot more sense than /item or something - this is pretty common sense but you’d be surprised how many people choose confusing names.
2. Data types: You should know what data is being sent and received with each API. For an endpoint that creates new Tweets, you might be sending a user ID (integer) and some Tweet content (string), and you might be receiving a Tweet ID (integer) and maybe some response codes. This isn’t a coding interview, so keep it simple.
3. HTTP methods: You should know what HTTP methods are used with each endpoint. For an endpoint that creates new Tweets, you would use a POST method, whereas an endpoint that fetches Tweets would be a GET method.

For our Twitter walkthrough, this is what we came up with:

Users need to be able to post tweets:

```
POST /tweet
Request
{
  “user_id”: “string”,
  "content": "string"
}
Response
{
  "tweet_id": "string",
  "status": "string"
}
```

Users need to be able to view individual tweets:

```
GET /tweet/<id>
Response
{
  "tweet_id": "string",
  "user_id": "string",
  "content": "string",
  "likes": "integer",
  "comments": "integer"
}
```

Users need to be able to view feed:

```
GET /feed
Response
[
  {
    "tweet_id": "string",
    "user_id": "string",
    "content": "string",
    "likes": "integer",
    "comments": "integer"
  }
]
```

Users need to be able to follow other users:

```
POST /follow
Request
{
  "follower_id": "string",
  "followee_id": "string"
}
Response
{
  "status": "string"
}
```

Users need to be able to like tweets:

```
POST /tweet/like
Request
{
  "tweet_id": "string",
  "user_id": "string"
}
Response
{
  "status": "string"
}
```

Users need to be able to comment on tweets:

```
POST /tweet/comment
Request
{
  "tweet_id": "string",
  "user_id": "string",
  "comment": "string"
}
Response
{
  "comment_id": "string",
  "status": "string"
}
```
