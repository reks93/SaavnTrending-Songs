# SaavnTrending-Songs
Build a system that keeps the users notified about new releases based on their music preferences
Saavn Trending Songs – Map reduce AssignmentAlgorithm
Classes in the Project:
1. Saavndriver.java - Main driver class for the job
2. Saavnmapper.java – The Mapper class
3. Saavnreducer.java – The Reducer Class
4. SaavnCombiner – The Combiner Class
5. SaavnPartitioner – The Partitioner Class
Mapper :
Key : Song id
Value : date
Input file is split by “,” and storing the line in a String array.
Songid is retrieved from the array[0] and date is retrieved by further splitting with "-".
Length of the songid is checked to be greater than 7 to exclude the (null) values present in song id.
Combiner:
Key : Song id
Value : {sum of the song ids}
I have implemented the combiner class separately by setting
job.setCombinerClass(SaavnCombiner.class) in the driver class to use the input and group together to
reduce the communication cost.
Reducer:
Key : Song id
Value : Total number of song streaming for that particular day
Counts the number of values associated with a key songid date.
sum is initialized to zero. for each key sum is calculated within the loop and the sum is returned.
writes the output to the file with count for each day w.r.t the songid/day. I have implemented 31 reducers
to get the output files per day from 1st to 31st Dec because of the huge size of the file. I have not set the no
of reducers in the driver class but passing it manually through the command prompt with -D
mapreduce.job.reduces=31
Partitioner:
Implemented the partitioner in such a way that it outputs the songid and count in a file for each day from
1
st to 31st Dec. It gets the day from the Mapper and once the day is validated with the Hash Map , the
custom partitioner sends the songid , count to the respective reducer (from 0 to 30)
Post Processing the data to find the trending song.
I have used Excel to find the average of the past 3 days to find the top 100 trending songs for nth day.
First got the top 1000 songs for each day from 22nd Dec to 31st Dec ,
 Took 3 days window time to find the trending songs for nth day. Then with the pivot table, found
out the top songs for each day by considering the following things.
 Created a table structure of Songid,date,Count (Taken as the pivot table fields) for days from 22nd
to 31st
 Song id is taken as the row for pivot and day as column. Taken the average of the sum of the days
count which is available in the Grand Total field in the picture.
 Have filtered the dates using the column labels based on the date. Hence if we select the date
from the Filter field, the top 100 songs are given as the output in the pivot.
 If a song count is blank for a particular day in the pivot , then it is neglected because the song was
not streamed on that particular day, hence it cannot be trending for the n
th day.
 If the average is less than the current day count, then the song is trending and is not a classic
because it has a ~more or less constant number of streaming on all the days( assumption ).
 Made sure that the spikes are not taken into account in the top 100 hence took 3 days of data /
song and analyzed the stream patterns to neglect the same.
 Used filters to make sure the data s for other days are not entered manually , but easily selected
from the filters column.
