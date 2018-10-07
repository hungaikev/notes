# Cargo Tracking. 

We’ll create a new package under src/main/scala and call it com.showtix. Scala uses packages to create namespaces which allow you to modularize programs.

- There is code that processes things coming from outside your service, makes sense of these facts, and then records them accurately in your event stream.
- There is the code that processes those recorded events and decides how each (in the order they were inserted) changes your eventual read model.
- And then there is the code that decides what to do if those events change the state of your read model in particular ways.

The application in question is responsible for ingesting real-time device data, and aggregating according to a hierarchy owned by a separate application. We will refer to this application as Real-Time Aggregator (RTA). The application is Cluster Sharded, where each of the nodes fall under one of the following roles:

1. Asset Shard: Responsible for receiving the raw data, and aggregating up the hierarchy.The aggregation is performed via actor messages through the cluster.

2. Kafka Consumer: Consume raw data from Kafka, and send it to the Asset Shard roles

IoT Platform is a fast and reliable way to make it happen. Utilizing its components for IoT enablement, logistics providers can achieve high levels of operational efficiency in regard to fleet management, cargo integrity monitoring, and automated warehousing operations.

1. Real-time fleet management
2. Smart labels
3. Predictive maintenance
4. Storage conditions control
5. Cargo integrity monitoring
6. Optimized warehouse workloads
7. Inventory tracking & analytics
8. End-to-end visibility into delivery process

## **Cargo monitoring**

With the Platform highly portable IoT features and support of the latest sensor technologies, logistics providers and shipping container manufacturers can easily assemble smart containers that allow tracking location, safety, and storage climate conditions of the delivered goods. Also, the enterprise-level support of the IoT Platform can be effectively utilized to integrate all required embedded electronics into a production-ready unit.

- **Embed electronics into regular containers and track cargo en route from A to B**
- **Implement full-condition monitoring inside containers**
- **Use multi-sensor tags on items to detect possible theft or misuse**
- **Use smart labels to track the condition of an individual item**
- **Provide accurate shipping status for customers in real time**
- **Plan delivery with higher accuracy for leaner warehousing**

## **Fleet and asset management**

The Platform augments widely available fleet management capabilities with accurate telematics data collection across vehicles in real-time, thus enabling complete visibility into the location, productivity, and technical condition of every connected vehicle. As one of the most comprehensive solutions for end-to-end IoT data gathering and processing, The platform can be used as a full-service platform for IoT-powered fleet management solution of any scale.

- **Unify the view of vehicles, machines, and equipment across all locations**
- **Collect and analyze vehicle telematics to ensure proper performance**
- **Track and optimize traffic, parking, workloads, and delivery schedule**
- **Implement sensor-based predictive maintenance for trucks and vehicles**
- **Implement maintenance and repair planning solutions**
- **Ensure full visibility into the cost of delivery, comparative analytics**
- **Strengthen driver’s safety with fatigue detection systems and wearable applications**
- **Manage equipment firmware**

With its ever-increasing demand for smarter supply chain management solutions, the logistics & transportation industry has been a key force behind the IoT upswing.

While we continue to hear all of the hype about the growth of the internet of things (IoT) with analysts predicting 20 billion networked things by 2020, IoT is expected to generate more than 20 zettabytes, or 20 trillion gigabytes, of data by 2025 equaling 500 gigabytes per device. Much of this is driving down the cost and improving accessibility of new supply chain tracking and monitoring technologies, which are being closely looked at by the top global shipping lines, 3PLs, and major shippers alike. As the Global Supply chain matures technologically, so do the demands and customer expectations on the Logistics around it. Shipping time, warehousing space, inventory flow across borders and certainly having real time visibility into supply chain disruptions.

Tracking and monitoring technologies can assist you in delivering a superior product in terms of quality and consistency to your customer. The continuous monitoring of goods makes sure that your customer is never surprised when your product arrives that it is not in the best condition possible.

When disruptions happen, because they always do, it is always better to know sooner than later about a particular shipment or incident during transit. In the majority of cases it’s not the disruption that is the real issue, the problem is the fact that the shipper was not able to let their customer know about it in enough time to prevent the fallout from the end customer.

Reduce those instances drastically by having the data from every shipment at your fingertips, so you can give your customer options simply because you have more time to react.

Better security and condition monitoring can help reduce loss or shrinkage of products in the supply chain, including that resulting from theft and damage. When theft or damage occurs, electronic forensics can determine when and where the loss occurs, and who had custody of the shipment at the time which can lower insurance claims and premium costs.

When you have visibility you are better able to know what your most efficient trade lanes are at a granular level. Through Geo-fencing you can calculate how long your shipments sit in particular areas in your trade lanes. This information is quite valuable when you apply it to your operation and make the changes once you identify the source of your delays or shipping issues. Long term the shipping data collected from the devices can also help you identify trends in your business flow that will allow you to make smarter decisions in the future.

## The bottom line

Better visibility, security, and cargo condition monitoring in your supply chain has three short and long term effects:

1. Reduces operations cost and lowers risk *– bottom line growth*
2. Increases revenue and facilitates business growth *– top line growth*
3. Multiple intangible benefits such as brand protection, increased customer satisfaction and loyalty, and better infrastructure for future business growth

---

---

---

The logistics industry faces many challenges across the globe – from rising energy costs to increasing security concerns and growing competition. Ubiquitous cellular n​etworks and leading edge M2M and GPS technology are transforming the industry by improving efficiency and profitability while reducing loss and theft.

Gemalto's leading edge M2M technology and the cloud based Application Enablement Platform enable solutions that track and trace vehicles, goods, employees and mobile resources giving logistics companies and the entire supply chain automated solutions, transparency and more effective processes.

Industries of nearly every kind benefit from asset tracking not only because it prevents theft, loss, and damage, but because assets comprise such a large percentage of a company’s holdings that asset management is essential for meeting compliance and industry standard regulations. Asset tracking that delivers real-time data contributes to a healthy bottom line, and IoT devices and systems make asset tracking more accurate and reliable than ever before.

Streaming data from billions of devices provides valuable insight into dynamic markets helping to develop new business models and revenue streams.​

This evolving and complex ecosystem can be difficult for new IoT developers and device makers to navigate. Despite a wonderful idea, they may not know where to begin to connect smart objects and take advantage of the data they can communicate. And things can get even more complicated when dealing with a large fleet of devices, designed to work in remote locations for many years

## Pinpoint Location

The exact location of goods and even stolen vehicles can be pinpointed in real time using Gemalto Cinterion M2M Modules with advanced GPS capabilities. A wide variety of connectivity technologies and module sizes help ensure virtually anything can be tracked

## Monitoring & Alerts

Using a variety of sensors, details about the condition of objects and goods can be transmitted to supervisors in real time including the temperature of goods, humidity levels, shock or damage suffered, speed of travel and even video images. For cold chain monitoring and transporting highly sensitive items such as pharmaceutical supplies and medical samples, immediate alerts are crucial so that adjustments can be made to preserve cargo.

## Route optimization in real time

Gemalto Cinterion solutions enable advanced navigation capabilities that draw data from other smart city solutions to provide up-to-the-minute traffic information used to optimize route and distribution schedules. Streamlined scheduling and routes save time, money, resources and aggravation while allowing retailers to transform the last leg of delivery into a moment of excellence that helps build brand loyalty.

The truck-tracking solution is designed to monitor what is happening with the trucks, captures the input and output weight to define available capacity, in addition to identifying which silo and person will carry the load.

The data is then correlated against external information, such as weather, humidity, temperature and the driver’s data, providing customers with a much more accurate delivery time estimate.

Once the truck leaves the distribution point, an automatic message is sent to the customer, informing them about the load, weight and estimated time of arrival. If part of the delivery is returned, the invoicing can be automated depending on the actual load delivered. Also, through the sensors located on the trucks, an information repository is generated using IoT and blockchain, which tracks all the exchanges, stops and transactions made by each truck and its respective load, from the distribution point to the final customer.

This heightened level of transparency can help increase accountability between shippers and their customers, promoting the flow of business.

Visibility, transparency and security throughout the cycle

The proper handling and use of information on transactions and exchanges related to cargoes is key to the logistics and transportation industry. Therefore, our main objective with this solution is to provide transparency and security throughout the transport cycle.

The implementation of this type of blockchain and IoT solution in the cloud is an opportunity to access critical data on-demand and make more informed decisions for the benefit of business

Shipping is another environment where any delays caused by manual errors can result in spoilage and lost profits.

One of the biggest challenges in the logistics and transportation industry is the protection of its assets and cargoes.

## Alter this passage

Traditionally, supply chain transactions are completed manually, creating delays and a higher risk for recording error, which can cause differences between what was recorded and what was actually loaded. By digitizing this process using blockchain and IoT, the relevant information is captured directly from the sensors placed on the trucks, and entered onto the blockchain, creating a single, shared repository that all authorized participants can access and which can only be altered with consensus from all parties.

## Devices:

[https://www.camcode.com/asset-tags/iot-asset-management-tracking/](https://www.camcode.com/asset-tags/iot-asset-management-tracking/)