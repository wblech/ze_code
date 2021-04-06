For this projects I created the image named miro_ze.png.


	How are we going to receive all the location data from the couriers' app? What protocols, services, components we are going to use to proper receive the data, store it and be available to be used in other products.
        Should we create an API for it?
        Should we use some specific managed component to receive this information?
        Should we use queues, pub-sub mechanisms, serverless components?

First I would set a route53 for a fixed DNS. Since the API gateway can handle 10.000 request per seconds (documentation) at first
we wouldnt need a Load Balancer, but the exercise asks for a scalable solution and the load balancer fits this idea of scalability.

The pipeline starts at the API Gateway which will send the data to a Kinesis. The Kinesis would stream the data
for 3 different groups:

1) First group is related to a Lambda connected with a SNS, that would send messages to the client related to their order.
2) The second is a Lambda connected with a Elasticache. The Elasticache is conected with another Lambda and a API gateway so it is possible to get the fastest way as possible the last location of the couries;
3) The third and last group is a Lambda with a Aurora to store in a relational database de data from the courier so the Analytics Team can use this data for their models.



		While receiving this location data associed with order's information, imagine we need to ingest more information to it.
		How would we do this?
		In which layer that you have proposed in the previous answer?


I whould change the layer related to the kinesis stream. So the kinesis whould get more data. And if the parse of data is different I would create another group with a Lambda to get the event of kinesis and parse the data.

    We need to create an API to retrieve the last retrieved location from an order.
        How do you propose us to do this?
This is the second group created above

        While receiving location data, can you elaborate a solution to store the order's last location information?
Using the Elasticache

	If we need to notify the users about each order's status, how would you implement this while collecting the data?
I would do this with the first group above with the SNS
    How to store the data and how to make it available to be queried by our Data Analytics team?
This is the third group above using Aurora.
