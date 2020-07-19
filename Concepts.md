# Handle a product will millions of likes
## Simple Design
ProductID   RatingCount
1           22
2           344
This will cause write hotspot when millions of write come at the same time.
## Normalize your table
User ID productID Rating
22      1           1
33      1           2
44      1           3
Then develop a separate job which will take care of aggregating rating to get the average.
## To handle lot of write and lot of read (in case of Tweeter likes)
Use copy on write i.e. on write, create a new row, modify the data and then do atomic switch.
Note: Not sure if this really helps.
## Perform Random write on 1000 new column
ProductID   RatingCount1  RatingCount2  RatingCountN-1  RatingCountN         
1           24            82            52              92              
2           144           844           244             114                
- App layer will perform random selection to get RatingColumn index from 1 to N
- Then it will perform write on that column
- Since multiple writer will be writing to multiple column therefore write contention wll be reduced/avoid
- To read the total count, reader will always read 1 to N column
