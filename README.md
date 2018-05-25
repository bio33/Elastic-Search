# New Document# Elastic-Search
CSCI 5408 : assignment 1 : Compare Relational DB against Elastic Search

# After preprocessing

Run the following commands to run the search query on Insomnia(REST client):

Query 1: Find all buses for a particular Bus Stop

LINK :Get ec2-18-216-144-223.us-east-2.compute.amazonaws.com:9200/stops/_search

BODY : 
{
"query": {
"has_child" : {
"type" : "stops_stoptimes",
"query" : {
"match_phrase" : {
"name_stop": "bridge Terminal [to Hfx] and [lanes 3 & 4]"
}
}
}
}
}

# Query 2 Find buses between two time ranges

Link : Get ec2-18-216-144-223.us-east-2.compute.amazonaws.com:9200/stops/_search

Body : 
{
"query":
{
"has_child" :
{
"type" : "stops_stoptimes",
"query": {
"range" :
{
"arrival_time" :
{
"gte": "16:18:00",
"lte": "18:23:00",
"format": "HH:mm:ss"
}
}
}
}
}
}
}

# Query 3 : Find route information of a particular bus on a particular route

Link : Get ec2-18-216-144-223.us-east-2.compute.amazonaws.com:9200/stops/_search

Body :
{
"query":
{
"has_parent" :
{
"parent_type" : "stoptimes",
"query":
{
"bool": {
"should":
[
{ "match": { "route_id": "320-114" }},
{ "match": { "trip_headsign": "57 PORTLAND HILLS VIA PORTLAND ESTATES" }}
]
}
}
}
}
}
}
## Query 4 Find top 3 bus stops that are the busiest throughout the day in terms of bus routes. (Hint: The bus stops with high volume of bus routes and close time gaps would be considered as busiest).

Link : Get ec2-18-216-144-223.us-east-2.compute.amazonaws.com:9200/stops/_search

Body :
{
"size": 0,
"aggs": {
"group_by_ip": {
"terms": {
"field": "stop_id",
"size": 3
},
"aggs": {
"topHits": {
"top_hits": {
"size": 1
}
}
}
}
}
}
