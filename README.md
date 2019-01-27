# Assessing-NHL-award-winners-using-K-means

After the end of each NHL season, various awards are handed out to players that were considered outstanding in various categories. Whilst there are some awards that are decided upon raw stats, such as, total number of goals in a regular season (Maurice Richard trophy) and total number of points (goals plus assists) in a regular season (Art Ross trophy), there are others that are somewhat more subjective and a voting system is used to decide the winner. Ballots for most awards are cast by the Professional Hockey Writers' Association, including the Frank J. Selke trophy: "to the forward who best excels in the defensive aspects of the game" and the James Norris Memorial trophy: "to the defense player who demonstrates throughout the season the greatest all-round ability in the position". I have never been a big fan of a voting system, although in some cases the resulting winners can be very obvious. I thought it might be interesting to see that by using standard and advanced hockey statistics alone, if I could classify players into Tiers and by doing so, identify the winners of these two "voting-style" trophies with the hypothesis that a winner in a given year should be in the top Tier.

Data sets

The final data-set used is a combination of traditional and advanced player metrics. Traditional statistics concern metrics like goals and assists (total being known as points), plus-minus, penalty minutes and time on ice, whilst advanced player metrics deal more with player behavior and puck possession. Using Python's beautifulsoup library, I scraped more traditional statistics (such as goals, assists, points, etc.) from www.Hockey-reference.com, whilst the advanced metrics were provided by www.corsica.hockey. Advanced hockey statistics from corsica hockey starts in 2008, hence, I have data for nearly 2000 players, spanning 2008–2018. It should be noted that I only considered advanced statistical data for where the game situation is at even-strength. Data were cleaned for missing values and were passed into Pandas for analysis. Whilst, I plan on getting to it eventually, this data set only applies to skaters and not goalies. Goalie stats are very different from skater stats.

Responsibility

In my NHL player rating repost (https://github.com/ashleyjones-projects/NHL-Player-rating), I devised a player rating system using the data set described above, where various statistical parameters were weighted and summed and passed through a standard Sigmoid function, yielding rating values between 50 and 99.9 (recursively) for a given regular season; the higher the score, the better the player's performance. The rating system identifies four areas to which the ratings are devised, one of which is the "responsibility" parameter known as DEFN which accounts for ~35% of the final rating value (the others being productivity ~50%, stamina ~8 %, and other miscellaneous game features making up ~ 7%). Responsibility is simply the (on average) defensive responsibility assigned to a player combined with how well he performed those duties over the regular season. It is a mathematical algorithm of a few advanced statistics defined below:

CF%: CF stands for "Corsi For", which is the number of shots a player's team generated when that player was on the ice as opposed to "Corsi Against" (CA), the number of shots generated for the opposing team whilst that player was on the ice. CF% is simply CF / (CF + CA).

CFQoC: The Quality of Competition's average CF%. The higher the number, the higher the level of competition. A somewhat debated statistic regarding its interpretation, but I like it when it's used in context with OZS (see below).

CFQoT: This is the CF% of a player's line-mates. It indicates how a player contributed to the overall play compared to his team-mates. Usually a good indicator to see if a given player made the players around him better.

OZS: Offensive Zone Starts. The percentage of times that a given player started their shift in the offensive zone.

xGF%: The ratio of Expected goals for versus Expected goals against. Expected goals are simply the likelihood of a goal being scored from a given shot. It provides judgement on the quality of shots. Hence, an xG with a value of 0.2, means the shot should be a goal 20% of the time. Essentially, players with a high score here are involved in high quality chances.

The algorithm takes into account which zone a player is most likely to start his shift with more weight given to a low OZS (thus more defensive duties), multiplied with the quality of competing players on the ice at the same time. So a player who has a large overall score here has a higher degree of "responsibility". How well they deal with the responsibility is decided by the multiplying the combined factors of the CF%, CFQoT, and xGF%
