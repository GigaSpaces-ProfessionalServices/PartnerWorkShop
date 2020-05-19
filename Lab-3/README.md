# PartnerWorkShop 
## Lab-3: Deploy a machine learning pipeline
* load data , enrich , train the model and predict Participants will use various industry leading building blocks such as spark , kafka and in-memory processing

### In this lab you will:
#### 1. Create an InsightEdge product gsctl cluster
#### 2. Deploy an application which consumes flight delay data to make binary predictions (yes/no) about which flights are likely to get delayed.

**Before starting the lab please:**<br>
Right click on the following 2 links and click Save Link As ...
* [Flight Delay Zeppelin Notebook1](https://amoll.s3-eu-west-1.amazonaws.com/Notebook/Gigaspaces+Studio_1.FlightDelays.json)
* [Flight Delay Zeppelin Notebook2](https://amoll.s3-eu-west-1.amazonaws.com/Notebook/Gigaspaces+Studio_2.Flight+Delays+2.json)

Please perform the following steps:

1. Enter the GSCTL interactive cli:<br>
   `java -jar gsctl.jar`<br><br>
2. Change product type from xap to insightedge:<br>
   `config product-type use insightedge`<br><br>
3. Create InsightEdge product gsctl cluster<br>
   ![snapshot](Pictures/Picture1_ie.png)<br><br>
4. Run the following command:<br>
   `list-services`<br>
   This returns all the services with their URLs:<br>
   ![snapshot](Pictures/Picture4_ie.png)<br><br>
5. Change artifact-repo to FlightDelay artifact repo:<br>
   `config artifact-repo use https://amoll.s3-eu-west-1.amazonaws.com/Services`<br><br>
6. Deploy flight_delay space (statefull pu which will hold the flights data)<br>
   `deploy --type=stateful --memory=2048 flights_space flight-delay-0.1.jar`<br><br>
7. Open zeppelin and import the two notebooks you saved in the beginning of the lab<br>
   ![snapshot](Pictures/Picture1.png)<br><br>
8. Change space name to “flights_space” in insightedge_jdbc interpreter<br>
   ![snapshot](Pictures/Picture2.png)<br><br>
9. Run all zeppelin paragraphs (one by one) until you reach the Kafka paragraph.<br>
   Before continuing to the kafka section your zeppelin last paragraph should yield accuracy = 0.79<br>
   ![snapshot](Pictures/Picture3.png)<br><br>
   Flights_space should have the following number of records:<br>
   ![snapshot](Pictures/Picture4.png)<br><br>
10. Deploy flights_feeder (stateless pu which will read flights from space and send to kafka)<br>
   `deploy --type=stateless --property=feeder.flights.path=/home/ec2-user/data.csv --property=kafka.bootstrapServer=kafka-0.service.consul:9092 flights_feeder kafka-pers-feeder.jar`<br><br>
11. Continue running the kafka streaming paragraph and the remaining zeppelin paragraphs.<br><br>
12. Last paragraph view should look something like:<br>
   ![snapshot](Pictures/Picture5.png)<br><br>
13. You are done with the lab!!!<br>
    Once you finish reviewing your work **Please Destroy the cluster**:<br>
    `destroy`
    






