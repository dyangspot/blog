title: "Twitter Streaming example using Play"
date: 2015-12-14 23:46:49
tags: [scala, play]
---

```
package controllers

import play.api.Play
import play.api.libs.oauth.{OAuthCalculator, ConsumerKey, RequestToken}
import play.api.libs.ws.WS
import play.api.mvc._
import play.api.Play.current
import scala.concurrent.ExecutionContext.Implicits.global.

import scala.concurrent.Future

class Application extends Controller {
  final val TwitterStatus = "https://stream.twitter.com/1.1/statuses/filter.json"

  def index = Action {
    Ok(views.html.index("Your new application is ready."))
  }

  def tweets = Action.async {
    credentials map {
      case (consumerKey, requestToken) =>
        WS.url(TwitterStatus)
          .sign(OAuthCalculator(consumerKey, requestToken))
        .withQueryString("track" -> "reactive")
        .get
        .map { response =>
          Ok(response.body)
        }
    } getOrElse {
      Future {
        InternalServerError("Twitter credentials not match")
      }
    }
  }

    def credentials: Option[(ConsumerKey,RequestToken)] =
      for {
        consumerKey <- Play.configuration.getString("twitter.consumerKey")
        consumerSecret <- Play.configuration.getString("twitter.consumerSecret")
        token <- Play.configuration.getString("twitter.token")
        tokenSecret <- Play.configuration.getString("twitter.tokenSecret")
      } yield
        (ConsumerKey(consumerKey, consumerSecret),
        RequestToken(token, tokenSecret))




}

```