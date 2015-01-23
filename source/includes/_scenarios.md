# Sample scenarios

## List running games

To list running games, simply use <a href="#list-games-for-source"> this call</a>. Use <a href="#instamrkt-source-uuid"> INSTAMRKT_SOURCE_ID</a> as game source.

## Join a game (subscribe)

Pick a game from a <a href="#list-running-games">list of currently running games</a>, and send a <a href="#join-game">join game</a> request. Include a game uuid of a selected game in your request.

## List open pools

Once you've <a href="#join-a-game-subscribe">joined a game</a>, you can list pools that are opened for predictions for this game with  <a href="#list-my-open-pools-for-game">this request</a>.

## Place a prediction

Once you know in which pool the prediction is to be placed (e.g. having <a href="#list-open-pools">listed</a> them), perform this using <a href="#place-bet-in-betting-pool">this call</a>. Remember to include the pool uuid in your request.

## Check balance

If you want to see how you're currently doing, you can always check your <a href="#current-balance">current</a> and <a href="#available-balance">available</a> balance, as well as check <a href="#exposure">exposure</a>.