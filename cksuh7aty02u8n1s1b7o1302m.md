## Introducing ScreenView: The missing social platform for movie/tv show lovers

Do you love to watch movies? Do you love to watch tv and web series? Then, ScreenView is for you. When the Auth0 hackathon was announced, I along with my friend decided to build a platform for what we love most which led to ScreenView. Before getting started with ScreenView, let's talk about Auth0 first.

## What is Auth0?
[Auth0](https://auth0.com) is an authentication-as-a-service platform. It makes user authentication extremely simple. You don't have to worry about security or managing user/roles because Auth0 can handle all that for you. This project uses Auth0 as the authentication service provider.

## What is ScreenView?
ScreenView is a social platform for movie/tv series lovers where the users can share their activities and stay updated with their friends. In short, we can say it's like Facebook/Twitter but targeted for movie/tv series lovers.

## The problem
Traditional social platforms like Facebook/Twitter are not enough to share your media activities. Yes, you can share what you're watching with your friends, but those posts will get lost under a pile of other generalized posts. Features like movie reviews, recommendations and discovery are not available on such platforms although these are essential. It's tough to find people with similar watching interests because those platforms aren't simply built for a specific audience.

## The solution
ScreenView. It lets all your dream come true. Everything you need for such a social platform is already there. This is built by movie lovers, for movie lovers. You can do the following:

- Discover upcoming, new and trending movies to watch
- Follow people with similar interests
- Stay up to date with what your friends are watching
- Share your opinion in the comments
- Create and keep track of your own Watchlist
- Share your Watchlist with your friends
- Write and share movie reviews with your friends
- And a lot more interesting features planned for the future

## The building blocks of ScreenView
| Type                            |                    Name                  |
| ----------------------  | -----------------------------|
| Language                  | Javascript/Node.js                   |
| Framework (Backend API)                  | Express                                  |
| Framework (Frontend)                  | React|
| Database                  | MongoDB                                  |
| ODM                  | Mongoose                                         |
| CSS Framework                  | Tailwind CSS |
| Auth Provider                  | Auth0                                  |
| Movie API Provider                 | TMDB                                  |


### Challenges we faced
This project was definitely technically challenging. There were many challenges that we faced. The most noteworthy one was **determining the business logic to show user feeds**. I had to find a way to effectively fetch and show user feeds in record time. After much hard work, I completed the job with success and full satisfaction. For those who are interested, you can look into the MongoDB query in the code repo (given at the end of the article).


## Quick sneak-peak

There are many features but to keep the article short I'm going to show you the main ones quickly.

In the below GIF, you can see the user posting a review of a movie. Remember, you can also post watch status, where you share what you're watching with your friends.
![sv-review-post.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1629991414350/hPnVzsZgC.gif)

In the next GIF, you see typical user activities like browsing through feeds, commenting, looking into the movie discovery section and user connections
![sv-feed-connection.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1629991823763/pUxl1qbaF.gif)


And in the final GIF, you'll see how easy it is to add items to your watchlist from anywhere. Interested in a movie that your friend is watching, just add it to the Watchlist right from the feed. Or interested in a new movie you discovered, just add it.
![sv-watchlist.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1629992251352/4dRkckXk6.gif)

## Try it out!

We're live: https://screenview.netlify.app

Code repository: https://github.com/ahmedsadman/screenview

Please note, what you see in your feed depends on the people you follow. This is a new site so it might take some time to gain traction. Until then, you can invite your friends to this site to grow your feed.

Hope you liked this work. Please don't forget to share it with your friends and family who loves screen entertainment. Thank you!