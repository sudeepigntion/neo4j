# neo4j


Neo4j queries

1. return all values
	MATCH (n) RETURN n

2. Return only player nodes
	MATCH (n:PLAYER) RETURN n

3. Return all Coaches
	MATCH (coach:COACH) RETURN coach

4. To return specific property
	match (player:PLAYER) return player.name, player.height

5. Alias
	match (player:PLAYER) return player.name as name, player.height as height

6. where clause
	match (player:PLAYER) WHERE player.name = "LeBron James" return player
	match (player:PLAYER {name: "LeBron James"}) return player

7. not equals to
	match (player:PLAYER) where player.name <> "LeBron James" return player

8. greater than 
	match (player:PLAYER) where player.height > 2 return player

	match (player:PLAYER) where (player.weight / (player.height * player.height)) > 25 return player

9. and and or clause
	match (player:PLAYER) where player.weight >= 100 and player.height < 3 return player 

10. skip and limit 
	match (player:PLAYER) return player skip 0 limit 5

11. order by
	match (player:PLAYER) return player order by player.name asc

12. multiple return 
	match (player:PLAYER), (coach:COACH) return player, coach

13. RelationShips
	match (player:PLAYER) -[:PLAYS_FOR] -> (team:TEAM) where team.name = "LA Lakers" return player,team

14. Contracts 
	match (player:PLAYER) -[contract:PLAYS_FOR] -> (team:TEAM) where contract.salary >= 3500000 return player	

15. match (player:PLAYER) -[contract:PLAYS_FOR] -> (team:TEAM) where contract.salary >= 35000000 return player, team

16. Find who is team mate of lebron James
	match (lebron:PLAYER {name: "LeBron James"}) -[:TEAMMATES] -> (teammate:PLAYER) return teammate

17. match (lebron:PLAYER {name: "LeBron James"}) -[:TEAMMATES] -> (teammate:PLAYER) 
match (teammate) -[contract:PLAYS_FOR] -> (team:TEAM)
where contract.salary >= 40000000
return teammate

18. Aggregation
	match (player:PLAYER) -[gamesPlayed:PLAYED_AGAINST] -> (:TEAM) return player.name as name, COUNT(gamesPlayed) as gamesPlayed, avg(gamesPlayed.points)

19. Delete Nodes if there is relationships
	match (ja {name: "Ja Morant"})
	detach delete ja

20. To delete relationships
	match (joel {name:"Joel Embiid"}) -[rel:PLAYS_FOR] -> (:TEAM)
	delete rel

21. To delete all nodes
	match (n) detach delete n

22. To add new data
	create (lebron:PLAYER:COACH:GENERAL_MANAGER {name:"LeBron James", height:2.01, weight:100})
	return lebron

23. Create relationships
	create(:PLAYER) -[:PLAYS_FOR {salary:34000000}]-> (:TEAM {name: "LA Lakers"})

24. update relationships
	match (lebron:PLAYER {name: "leBron James"}), (lakers:TEAM {name: "LA Lakers"})
	create (lebron) -[:PLAYS_FOR {salary: 40000000}]->(lakers)

25. update properties
	match (anthony:PLAYER) where ID(anthony) = 1 set anthony.name = "Anthony Davis" return anthony

26. update and new properties
	match (lebron:PLAYER) where ID(lebron) = 0 set lebron.height=3.02, lebron.age=42 return lebron

27. Get all labels
	match (lebron:PLAYER) where ID(lebron) = 0 set lebron:REF return lebron

28. update properties of relationships
	match (lebron {name:"LeBron James"}) -[contract:PLAYS_FOR] -> (team:TEAM)
	set contract.salary = 100000000
	return lebron, team

29. remove properties
	match (lebron {name: "LeBron James"})
	remove lebron:GENERAL_MANAGER return lebron

	match (lebron {name: "LeBron James"})
	remove lebron.age return lebron
