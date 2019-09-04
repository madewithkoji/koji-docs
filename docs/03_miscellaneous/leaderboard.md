# Leaderboard Implementation Guide

Koji is place full of interaction. Creators create games and fun webapps that make our life a beautiful place.

Koji apps and games are much more fun if they have Leaderboards that create a competition like atmosphere among the users that use the apps.

These are few games on Koji, that have Leaderboards.

- @Svarog's [Element Match Game](https://withkoji.com/~Svarog1389/element-match)
- @Svarog's [Brick Breaker Game](https://withkoji.com/~Svarog1389/brick-breaker)
- @kumar_abhirup's [Loon Shooter Game](https://withkoji.com/~kumar_abhirup/loon-shooter)

This documentation contains the guide to implement the Leaderboard in your game or app, in just few easy steps!

## Libraries we'll use

- [koji-leaderboard-api](https://www.npmjs.com/package/koji-leaderboard-api)
- [koji-react-leaderboard](https://www.npmjs.com/package/koji-react-leaderboard)

## Getting Started

To contain the Leaderboard data, you need a Database. You could have used any database, but **we'll use the Koji Database because it is the easiest to implement on Koji.**

To work with Database, you also need a backend. The backend will connect you to the database when its API endpoint is pinged by your frontend.

For doing easy setup, we'll make use of [koji-leaderboard-api](https://www.npmjs.com/package/koji-leaderboard-api) NPM Package.

## Let's get started!

### Build the Backend

To set [koji-leaderboard-api](https://www.npmjs.com/package/koji-leaderboard-api) in your backend, you need to spin up an Express server.

#### Do the following in your `backend/index.js` 👇
```js
import express from 'express'
import bodyParser from 'body-parser'

import kojiLeaderboardApi from 'koji-leaderboard-api' // The library you are using

const app = express() // This is the Express App Instance

// Body parser
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({
  limit: '2mb',
  extended: true,
}))

// CORS
app.use((req, res, next) => {
  res.header('Access-Control-Allow-Origin', '*') // use '*' as second param to allow any client to hack in
  res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept, Authorization, X-Jiro-Request-Tag')
  next()
})

/**
 * @name kojiLeaderboardApi
 * @description Doing `kojiLeaderboardApi(app)` activates the `/leaderboard` GET and POST API endpoints
 *              that your frontend can use to Display and Update Leaderboard
 * 
 * @param {Express App Instance} app - (required)
 * @param {String} tableName - (optional) Default: `leaderboard`
 */
kojiLeaderboardApi(app)

// Listen on Port 8080. Visit http://localhost:8080 to see the backend.
app.listen(8080, null, async err => {
    if (err) console.log(err.message)
    console.log('[koji] Backend Started 👏')
})
```

Yes! Just doing that will set up the Koji Leaderboard API in your backend. For more info on `koji-leaderboard-api` and its use, read [this documentation](https://www.npmjs.com/package/koji-leaderboard-api).

#### API Endpoints

|   | **Method** | **Endpoint**   | **Parameters (JSON Body)**                                                                             |
|---|------------|----------------|--------------------------------------------------------------------------------------------------------|
| 1 | GET        | `/leaderboard` |                                                    -                                                   |
| 2 | POST       | `/leaderboard` | name: String 👈 _required_ <br /> score: Number 👈 _required_ <br /> privateAttributes: Object 👈 _optional_ |

> The above mentioned API Endpoints get activated by using `koji-leaderboard-api`. You can use those endpoints, to get the leaderboard data and also to save some data to the leaderboard.

#### Connect Koji Database from outside Koji

**The above API Endpoints will only work in the Koji Published Apps** because the `PROJECT_ID` and `PROJECT_TOKEN` environment variables are already filled there.

However, if you want the backend to connect to the Koji Database on `http://localhost`, or on any other platform, you'll need those environment variables.

For that, you'll have to use the [dotenv npm package](http://npmjs.com/package/dotenv).

To get all your environent variables, open the terminal in the Koji Editor and type `env` and hit enter. Copy all the result you get and paste that in your `.env` file which is connected to your backend with the help of `dotenv` package.

### Build the frontend

**If you are done building the backend, you are halfway done! The API Endpoints can now be pinged from any frontend!**

To learn about fetching data from Koji Leaderboard API endpoints, [read this](https://github.com/KumarAbhirup/koji-leaderboard-api#get-leaderboard).

#### If you are using React, things are made easy for you!

To fetch leaderboard data from the backend API endpoints in React, you don't have to do complicated asyncronous browser fetches. The [koji-react-leaderboard](http://npmjs.com/package/koji-react-leaderboard) component library handles it all for you!

You can implement the Leaderboard in React all by yourself by reading [this documentation](https://github.com/KumarAbhirup/koji-react-leaderboard#%EF%B8%8F-usage).

Install the `koji-react-leaderboard` package and see how easy it is to **get the Leaderboard data**. 👇

```javascript
import React, { Component } from 'react'
import Koji from '@withkoji/vcc'

import { GetLeaderboard } from 'koji-react-leaderboard'

class YourComponent extends Component {
  render () {
    return (
      <GetLeaderboard kojiLeaderboardBackendUri={Koji.config.serviceMap.backend}>
        {(data, isLoading, isError) => {
          // that's it! 
          // The `data` contains all the information you need.
          // `isLoading` is a boolean that tells whether the data is loaded.
          // `isError` is a boolean that tells whether there is some error.
        }}
      </GetLeaderboard>
    )
  }
}
```

**Save data to Koji Leaderboard** 👇

```javascript
import React, { Component } from 'react'
import Koji from '@withkoji/vcc'

import { SaveToLeaderboard } from 'koji-react-leaderboard'

class YourComponent extends Component {
  state = { data: null }
  
  submitData = async (e, saveData) => {
    e.preventDefault()

    const response = await saveData({
      name: 'ScreamerPlays',
      score: 890,
    })

    this.setState({ data: response })
  }

  render() {
    return (
      <SaveToLeaderboard kojiLeaderboardBackendUri={Koji.config.serviceMap.backend}>
        {(saveData, isLoading, isError) => {
          // On Click the button saves the data
          // Use the `saveData` as a function to save the data to the leaderboard.
          return (
            <React.Fragment>
              <button onClick={e => this.submitData(e, saveData)}>Submit data</button>
              {isLoading && <h2>Loading...</h2>}
              {isError && <h2>Error</h2>}
            </React.Fragment>
          )
        }}
      </SaveToLeaderboard>
    )
  }
}
```

In both the above examples, you see the use of `Koji.config.serviceMap.backend` as the backend URL. Yes, with `@withkoji/vcc` package, you get the Koji backend URL which works on staging as well as on production. So, instead of hardcoding the backend URL, make use of `@withkoji/vcc`.

## Links

- `koji-leaderboard-api` NPM Package 👉 [http://npmjs.com/package/koji-leaderboard-api](http://npmjs.com/package/koji-leaderboard-api)

- `koji-leaderboard-api` GitHub Repo 👉 [https://github.com/KumarAbhirup/koji-leaderboard-api](https://github.com/KumarAbhirup/koji-leaderboard-api)

- `koji-react-leaderboard` NPM Package 👉 [http://npmjs.com/package/koji-react-leaderboard](http://npmjs.com/package/koji-react-leaderboard)

- `koji-react-leaderboard` GitHub Repo 👉 [https://github.com/KumarAbhirup/koji-react-leaderboard](https://github.com/KumarAbhirup/koji-react-leaderboard)

> The libraries are made in TypeScript. `koji-react-leaderboard` library uses React Hooks and TypeScript. All kinds of open source contributions to both the libraries are accepted.

## Conclusion

Including leaderboard in your Koji app can be fun. 

If you have any queries regarding the leaderboard implementation, ask it in the **#support** channel of Koji's Official Discord Server. @koji-team will help. :) Peace.