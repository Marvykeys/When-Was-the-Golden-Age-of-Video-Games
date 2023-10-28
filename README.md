# :video_game: When-Was-the-Golden-Age-of-Video-Games :video_game:

<p>In this project, I explore the top 400 best-selling video games created between 1977 and 2020. I'll compare a dataset on game sales with critic and user reviews to determine whether or not video games have improved as the gaming market has grown.</p>
<p>The database contains two tables. But you can find the complete dataset with over 13,000 games on <a href="https://www.kaggle.com/holmjason2/videogamedata">Kaggle</a>. </p>
<h3 id="game_sales"><code>game_sales</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>game</code></td>
<td>varchar</td>
<td>Name of the video game</td>
</tr>
<tr>
<td style="text-align:left;"><code>platform</code></td>
<td>varchar</td>
<td>Gaming platform</td>
</tr>
<tr>
<td style="text-align:left;"><code>publisher</code></td>
<td>varchar</td>
<td>Game publisher</td>
</tr>
<tr>
<td style="text-align:left;"><code>developer</code></td>
<td>varchar</td>
<td>Game developer</td>
</tr>
<tr>
<td style="text-align:left;"><code>games_sold</code></td>
<td>float</td>
<td>Number of copies sold (millions)</td>
</tr>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Release year</td>
</tr>
</tbody>
</table>
<h3 id="reviews"><code>reviews</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>game</code></td>
<td>varchar</td>
<td>Name of the video game</td>
</tr>
<tr>
<td style="text-align:left;"><code>critic_score</code></td>
<td>float</td>
<td>Critic score according to Metacritic</td>
</tr>
<tr>
<td style="text-align:left;"><code>user_score</code></td>
<td>float</td>
<td>User score according to Metacritic</td>
</tr>
</tbody>
</table>
<p>Let's begin by looking at some of the top selling video games of all time!</p>

## 1. The ten best-selling video games

```python 
SELECT *
FROM game_sales
ORDER BY games_sold DESC
LIMIT 10;
```
<table>
    <thead>
        <tr>
            <th>game</th>
            <th>platform</th>
            <th>publisher</th>
            <th>developer</th>
            <th>games_sold</th>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Wii Sports for Wii</td>
            <td>Wii</td>
            <td>Nintendo</td>
            <td>Nintendo EAD</td>
            <td>82.90</td>
            <td>2006</td>
        </tr>
        <tr>
            <td>Super Mario Bros. for NES</td>
            <td>NES</td>
            <td>Nintendo</td>
            <td>Nintendo EAD</td>
            <td>40.24</td>
            <td>1985</td>
        </tr>
        <tr>
            <td>Counter-Strike: Global Offensive for PC</td>
            <td>PC</td>
            <td>Valve</td>
            <td>Valve Corporation</td>
            <td>40.00</td>
            <td>2012</td>
        </tr>
        <tr>
            <td>Mario Kart Wii for Wii</td>
            <td>Wii</td>
            <td>Nintendo</td>
            <td>Nintendo EAD</td>
            <td>37.32</td>
            <td>2008</td>
        </tr>
        <tr>
            <td>PLAYERUNKNOWN&#x27;S BATTLEGROUNDS for PC</td>
            <td>PC</td>
            <td>PUBG Corporation</td>
            <td>PUBG Corporation</td>
            <td>36.60</td>
            <td>2017</td>
        </tr>
        <tr>
            <td>Minecraft for PC</td>
            <td>PC</td>
            <td>Mojang</td>
            <td>Mojang AB</td>
            <td>33.15</td>
            <td>2010</td>
        </tr>
        <tr>
            <td>Wii Sports Resort for Wii</td>
            <td>Wii</td>
            <td>Nintendo</td>
            <td>Nintendo EAD</td>
            <td>33.13</td>
            <td>2009</td>
        </tr>
        <tr>
            <td>Pokemon Red / Green / Blue Version for GB</td>
            <td>GB</td>
            <td>Nintendo</td>
            <td>Game Freak</td>
            <td>31.38</td>
            <td>1998</td>
        </tr>
        <tr>
            <td>New Super Mario Bros. for DS</td>
            <td>DS</td>
            <td>Nintendo</td>
            <td>Nintendo EAD</td>
            <td>30.80</td>
            <td>2006</td>
        </tr>
        <tr>
            <td>New Super Mario Bros. Wii for Wii</td>
            <td>Wii</td>
            <td>Nintendo</td>
            <td>Nintendo EAD</td>
            <td>30.30</td>
            <td>2009</td>
        </tr>
    </tbody>
</table>


## 2. Missing review scores
<p>Wow, the best-selling video games were released between 1985 to 2017! That's quite a range; we'll have to use data from the <code>reviews</code> table to gain more insight on the best years for video games. </p>
<p>First, it's important to explore the limitations of our database. One big shortcoming is that there is not any <code>reviews</code> data for some of the games on the <code>game_sales</code> table. </p>


```python
SELECT COUNT (*)
FROM game_sales AS g_s
FULL JOIN reviews AS r
ON g_s.game = r.game
WHERE critic_score IS NULL AND user_score IS NULL;
```

<table>
    <thead>
        <tr>
            <th>count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>31</td>
        </tr>
    </tbody>
</table>

## 3. Years that video game critics loved
<p>It looks like a little less than ten percent of the games on the <code>game_sales</code> table don't have any reviews data. That's a small enough percentage that we can continue our exploration, but the missing reviews data is a good thing to keep in mind as we move on to evaluating results from more sophisticated queries. </p>
<p>There are lots of ways to measure the best years for video games! Let's start with what the critics think. </p>

```python
SELECT g_s.year AS year, 
       ROUND(AVG(r.critic_score),2) AS avg_critic_score 
FROM game_sales AS g_s
JOIN reviews AS r
ON g_s.game = r.game
GROUP BY year
ORDER BY avg_critic_score DESC
LIMIT 10;
```

<table>
    <thead>
        <tr>
            <th>year</th>
            <th>avg_critic_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1990</td>
            <td>9.80</td>
        </tr>
        <tr>
            <td>1992</td>
            <td>9.67</td>
        </tr>
        <tr>
            <td>1998</td>
            <td>9.32</td>
        </tr>
        <tr>
            <td>2020</td>
            <td>9.20</td>
        </tr>
        <tr>
            <td>1993</td>
            <td>9.10</td>
        </tr>
        <tr>
            <td>1995</td>
            <td>9.07</td>
        </tr>
        <tr>
            <td>2004</td>
            <td>9.03</td>
        </tr>
        <tr>
            <td>1982</td>
            <td>9.00</td>
        </tr>
        <tr>
            <td>2002</td>
            <td>8.99</td>
        </tr>
        <tr>
            <td>1999</td>
            <td>8.93</td>
        </tr>
    </tbody>
</table>

## 4. Was 1982 really that great?
<p>The range of great years according to critic reviews goes from 1982 until 2020: we are no closer to finding the golden age of video games! </p>
<p>Hang on, though. Some of those <code>avg_critic_score</code> values look like suspiciously round numbers for averages. The value for 1982 looks especially fishy. Maybe there weren't a lot of video games in our dataset that were released in certain years. </p>
<p>Let's update our query and find out whether 1982 really was such a great year for video games.</p>


```python
SELECT COUNT(g_s.game) AS num_games,
       g_s.year AS year, 
       ROUND(AVG(r.critic_score),2) AS avg_critic_score 
FROM game_sales AS g_s
JOIN reviews AS r
ON g_s.game = r.game
GROUP BY year
HAVING COUNT(g_s.game) > 4
ORDER BY avg_critic_score DESC
LIMIT 10;
```

<table>
    <thead>
        <tr>
            <th>num_games</th>
            <th>year</th>
            <th>avg_critic_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>10</td>
            <td>1998</td>
            <td>9.32</td>
        </tr>
        <tr>
            <td>11</td>
            <td>2004</td>
            <td>9.03</td>
        </tr>
        <tr>
            <td>9</td>
            <td>2002</td>
            <td>8.99</td>
        </tr>
        <tr>
            <td>11</td>
            <td>1999</td>
            <td>8.93</td>
        </tr>
        <tr>
            <td>13</td>
            <td>2001</td>
            <td>8.82</td>
        </tr>
        <tr>
            <td>26</td>
            <td>2011</td>
            <td>8.76</td>
        </tr>
        <tr>
            <td>13</td>
            <td>2016</td>
            <td>8.67</td>
        </tr>
        <tr>
            <td>18</td>
            <td>2013</td>
            <td>8.66</td>
        </tr>
        <tr>
            <td>20</td>
            <td>2008</td>
            <td>8.63</td>
        </tr>
        <tr>
            <td>12</td>
            <td>2012</td>
            <td>8.62</td>
        </tr>
    </tbody>
</table>

## 5. Years that dropped off the critics' favorites list
<p>That looks better! The <code>num_games</code> column convinces us that our new list of the critics' top games reflects years that had quite a few well-reviewed games rather than just one or two hits. But which years dropped off the list due to having four or fewer reviewed games? Let's identify them so that someday we can track down more game reviews for those years and determine whether they might rightfully be considered as excellent years for video game releases!</p>
<p>To get started, we've created tables with the results of our previous two queries:</p>
<h3 id="top_critic_years"><code>top_critic_years</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Year of video game release</td>
</tr>
<tr>
<td style="text-align:left;"><code>avg_critic_score</code></td>
<td>float</td>
<td>Average of all critic scores for games released in that year</td>
</tr>
</tbody>
</table>
<h3 id="top_critic_years_more_than_four_games"><code>top_critic_years_more_than_four_games</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Year of video game release</td>
</tr>
<tr>
<td style="text-align:left;"><code>num_games</code></td>
<td>int</td>
<td>Count of the number of video games released in that year</td>
</tr>
<tr>
<td style="text-align:left;"><code>avg_critic_score</code></td>
<td>float</td>
<td>Average of all critic scores for games released in that year</td>
</tr>
</tbody>
</table>


```python
SELECT year, 
       avg_critic_score
FROM top_critic_years
EXCEPT 
SELECT year, 
       avg_critic_score
FROM top_critic_years_more_than_four_games
ORDER BY avg_critic_score DESC;
```

<table>
    <thead>
        <tr>
            <th>year</th>
            <th>avg_critic_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1990</td>
            <td>9.80</td>
        </tr>
        <tr>
            <td>1992</td>
            <td>9.67</td>
        </tr>
        <tr>
            <td>2020</td>
            <td>9.20</td>
        </tr>
        <tr>
            <td>1993</td>
            <td>9.10</td>
        </tr>
        <tr>
            <td>1995</td>
            <td>9.07</td>
        </tr>
        <tr>
            <td>1982</td>
            <td>9.00</td>
        </tr>
        </tr>
    </tbody>
</table>

## 6. Years video game players loved
<p>Based on our work in the task above, it looks like the early 1990s might merit consideration as the golden age of video games based on <code>critic_score</code> alone, but we'd need to gather more games and reviews data to do further analysis. </p>
<p>Let's move on to looking at the opinions of another important group of people: players! To begin, let's create a query very similar to the one we used in Task Four, except this one will look at <code>user_score</code> averages by year rather than <code>critic_score</code> averages.</p>

```python
SELECT COUNT(g_s.game) AS num_games,
       g_s.year AS year, 
       ROUND(AVG(r.user_score),2) AS avg_user_score 
FROM game_sales AS g_s
JOIN reviews AS r
ON g_s.game = r.game
GROUP BY year
HAVING COUNT(g_s.game) > 4
ORDER BY avg_user_score DESC
LIMIT 10;
```

<table>
    <thead>
        <tr>
            <th>num_games</th>
            <th>year</th>
            <th>avg_user_score</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>8</td>
            <td>1997</td>
            <td>9.50</td>
        </tr>
        <tr>
            <td>10</td>
            <td>1998</td>
            <td>9.40</td>
        </tr>
        <tr>
            <td>23</td>
            <td>2010</td>
            <td>9.24</td>
        </tr>
        <tr>
            <td>20</td>
            <td>2009</td>
            <td>9.18</td>
        </tr>
        <tr>
            <td>20</td>
            <td>2008</td>
            <td>9.03</td>
        </tr>
        <tr>
            <td>5</td>
            <td>1996</td>
            <td>9.00</td>
        </tr>
        <tr>
            <td>13</td>
            <td>2005</td>
            <td>8.95</td>
        </tr>
        <tr>
            <td>16</td>
            <td>2006</td>
            <td>8.95</td>
        </tr>
        <tr>
            <td>8</td>
            <td>2000</td>
            <td>8.80</td>
        </tr>
        <tr>
            <td>11</td>
            <td>1999</td>
            <td>8.80</td>
        </tr>
    </tbody>
</table>

## 7. Years that both players and critics loved
<p>Alright, we've got a list of the top ten years according to both critic reviews and user reviews. Are there any years that showed up on both tables? If so, those years would certainly be excellent ones!</p>
<p>Recall that we have access to the <code>top_critic_years_more_than_four_games</code> table, which stores the results of our top critic years query from Task 4:</p>
<h3 id="top_critic_years_more_than_four_games"><code>top_critic_years_more_than_four_games</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Year of video game release</td>
</tr>
<tr>
<td style="text-align:left;"><code>num_games</code></td>
<td>int</td>
<td>Count of the number of video games released in that year</td>
</tr>
<tr>
<td style="text-align:left;"><code>avg_critic_score</code></td>
<td>float</td>
<td>Average of all critic scores for games released in that year</td>
</tr>
</tbody>
</table>
<p>We've also saved the results of our top user years query from the previous task into a table:</p>
<h3 id="top_user_years_more_than_four_games"><code>top_user_years_more_than_four_games</code></h3>
<table>
<thead>
<tr>
<th style="text-align:left;">column</th>
<th>type</th>
<th>meaning</th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:left;"><code>year</code></td>
<td>int</td>
<td>Year of video game release</td>
</tr>
<tr>
<td style="text-align:left;"><code>num_games</code></td>
<td>int</td>
<td>Count of the number of video games released in that year</td>
</tr>
<tr>
<td style="text-align:left;"><code>avg_user_score</code></td>
<td>float</td>
<td>Average of all user scores for games released in that year</td>
</tr>
</tbody>
</table>


```python
SELECT year
FROM top_critic_years_more_than_four_games
INTERSECT 
SELECT year
FROM top_user_years_more_than_four_games;
```

<table>
    <thead>
        <tr>
            <th>year</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1998</td>
        </tr>
        <tr>
            <td>2008</td>
        </tr>
        <tr>
            <td>2002</td>
        </tr>
        <tr>
    </tbody>
</table>

## 8. Sales in the best video game years
<p>Looks like we've got three years that both users and critics agreed were in the top ten! There are many other ways of measuring what the best years for video games are, but let's stick with these years for now. We know that critics and players liked these years, but what about video game makers? Were sales good? Let's find out.</p>
<p>This time, we haven't saved the results from the previous task in a table for you. Instead, we'll use the query from the previous task as a subquery in this one! This is a great skill to have, as we don't always have write permissions on the database we are querying.</p>

```python
SELECT year,
       SUM(games_sold) AS total_games_sold
FROM game_sales
WHERE year IN (SELECT year
               FROM top_critic_years_more_than_four_games
               INTERSECT 
               SELECT year
               FROM top_user_years_more_than_four_games)
GROUP BY year
ORDER BY total_games_sold DESC;
```

<table>
    <thead>
        <tr>
            <th>year</th>
            <th>total_games_sold</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2008</td>
            <th>105.07</th>
        </tr>
        <tr>
            <td>1998</td>
            <th>101.52</th>
        </tr>
        <tr>
            <td>2002</td>
            <th>58.67</th>
        </tr>
        <tr>
    </tbody>
</table>



