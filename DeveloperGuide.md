# 📈 Developer Guide: Demand Forecasting & Trend Prediction 

As a developer partner on the our platform, you can easily implement inventory demand forecasting, AI-driven stock prediction, and trend analysis within your own AI agents, web dashboards, or mobile applications. 

Because we focuses purely on high-performance data persistence and standard CRUD operations, **you have full control over your forecasting stack**. The core database schemas expose everything your application needs to calculate future sales spikes, drops, or seasonal velocity. 

## 🧭 Core Fields for Time-Series Analysis 

To predict whether an item's sales will increase or decrease in the near future, your AI agent or application only needs to aggregate and track three primary fields from the **OrderDetailsEntity**

*   **createdAt:** Your time axis. Used to bucket sales into daily, weekly, or monthly data intervals. 

*   **itemName or lineId:** The unique identifier used to group data points per specific inventory item. 
  
*   **orderQuantity:** The metric or target variable you are trying to predict. 
--- 

## ️ 🛠️ Step-by-step Integration Workflow

*    **Step 1: Fetch Historical Order Data**
  
     Your application or autonomous AI agent should periodically make standard **GET** requests to pull historical data for the target store or item. 

     **HTTP** **GET**: op/apiv1/order-details/paginated?page=0&size=1000&sort=createdAt

*    **Step 2: Aggregate Records (Data Preparation)**

     Raw order lines from the database are transactional and arrive at random timestamps. Before feeding this data to any statistical tool, you must aggregate the quantities into
     uniform chronological buckets (e.g., Daily Sales). 

     Here is a quick **Node.js** / **JavaScript** snippet to clean and structure the payload received from our platform

    // 1. Mock data array representing the 'OrderDetailsEntity' payload from platform 

    const etsOrderRecords = [
    { createdAt: "2026-05-19T10:14:00Z", itemName: "Eco-Bottle 500ml", orderQuantity: 12 },
    { createdAt: "2026-05-19T15:30:00Z", itemName: "Eco-Bottle 500ml",orderQuantity: 8 },
    { createdAt: "2026-05-20T09:05:00Z", itemName: "Eco-Bottle 500ml", orderQuantity: 15 }]; 
 
    // 2. Reduce transactional lines into a clean timeline object 

    const structuredTimeline = etsOrderRecords.reduce((accumulator, order) => { 
    const dateOnly = order.createdAt.split('T')[0]; // Yields "YYYY-MM-DD" 
    accumulator[dateOnly] = (accumulator[dateOnly] || 0) + order.orderQuantity; 
    return accumulator; 
    }, {}); 
 
    console.log(structuredTimeline); 
    // Output ready for forecasting engine: { "2026-05-19": 20, "2026-05-20": 15 } 

*   **Step 3: Run the Forecasting Engine**

    Depending on your tech stack, you can instantly pass this clean timeline array into open-source mathematical libraries

    **For Mobile & Web UI Dashboards (Client-Side)**: Use lightweight tools like **regression-js** or simple-statistics to run rapid linear trend regressions. Plot these coordinates
    straight into charts (**Chart.js** or **ECharts**) to draw a dotted line into the future for your store managers. 

    **For Autonomous AI Agents (Python / Server-Side)**: Convert your timeline into a **Pandas DataFrame** and pass it to **Meta’s Prophet** library. It naturally extracts weekly/yearly
    seasonality and accounts for promotional holidays to tell your agent if a stockout risk is imminent. 

---

## 💡 Pro-Tip: Advanced Enterprise Forecasts 

   Don't limit your forecasts strictly to individual items. The **OrderDetailsEntity** schema provides deep structural categorization metadata you can exploit for macro-predictions 

*   **Department & Class Forecasting**: Group your history by **deptId**, **classId**, or **subclassId** to predict high-level purchasing shifts across entire merchandise domains
    instead of single items. 

*   **Logistics & Spatial Planning**: Multiply your calculated future item quantities by the **cube** (volume) and **weight** attributes. This enables your application to forecast
    exactly how much physical warehouse shelf space or shipping truck cargo capacity the store will require in the coming weeks. 
--- 

## 📈 Developer Guide Addendum: Advanced Supply Chain Forecasting 

By combining the **OrderHeaderEntity** with the underlying **orderDetailsEntities**, your applications and AI agents can transition from basic sales forecasting to highly advanced supply chain, shipping, and logistics prediction. 

---

### 🚚 Core Header Fields for Advanced Predictive Models 

While the order details tell you how much item inventory is moving, the header provides the physical constraints of time and geography. Developers can leverage these specific header fields


**Field Name** ---> **Supply Chain Predictive Use Case**


shipWeekStartDate  ---> **Macro-Seasonality**: Best for high-level weekly velocity mapping, skipping daily sales noise. 


originFacilityId ---> **Sourcing Forecast**: Predicts outbound volume strain on specific fulfillment centers. 


destinationFacilityId ---> **Geographic Demand**: Forecasts regional item popularity and localized stockouts. 


deliveryStartDttm / EndDttm ---> **Transit Analytics**: Helps AI models calculate expected lead times and window bottlenecks. 


scheduledDayOfWeek ---> **Day-of-Week Seasonality**: Predicts which days face the highest shipping and delivery spikes. 


orderFulfillmentStatus ---> **Data Filtering**: Ensures models filter out unfulfilled or pending entries for clean historical actuals. 


cancelled ---> **Anomaly Detection**: Allows AI agents to subtract canceled orders so they don't skew real demand figures. 

---

### 🛠️ Step-by-Step Integration Workflow (Parent-Child Schema) 

**Step 1: Extract and Filter Valid Order History**

When developer partners query our order header endpoint, they should filter out canceled orders to keep their forecasting data clean and accurate

**http** 

**GET** op/apiv1/order-details?orderHeaderId=152&isCancelled=false

**Step 2: Correlate Location and Timestamps (Data Prep)**

Because our platform structure nests orderDetailsEntities inside the OrderHeaderEntity, developers can map items directly to **specific geographic destination nodes and target delivery weeks**. 

Here is a **JavaScript/Node.js** template showing how developers can parse our nested structure to prepare a location-based forecasting dataset

**javascript**

// 1. Mock nested payload received from your BaaS platform 
const etsOrderHeaders = [ 
  { 
    orderHeaderId: 1001, 
    shipWeekStartDate: "2026-05-18T00:00:00Z", 
    destinationFacilityId: "FACILITY-EAST-01", 
    cancelled: false, 
    orderDetailsEntities: [{ itemName: "Eco-Bottle 500ml", orderQuantity: 50 },{ itemName: "Bamboo Straws", orderQuantity: 100 }] 
  }, 
  { 
    orderHeaderId: 1002, 
    shipWeekStartDate: "2026-05-18T00:00:00Z", 
    destinationFacilityId: "FACILITY-WEST-02", 
    cancelled: false, 
    orderDetailsEntities: [ 
      { itemName: "Eco-Bottle 500ml", orderQuantity: 30 } 
    ] 
  } 
]; 
 
// 2. Map items to specific destination facilities for regional forecasting 
const regionalDemandMatrix = {}; 
 
etsOrderHeaders.forEach(header => { 
  const weekStr = header.shipWeekStartDate.split('T')[0]; // "2026-05-18" 
  const location = header.destinationFacilityId; 
 
  header.orderDetailsEntities.forEach(item => { 
    if (!regionalDemandMatrix[location]) regionalDemandMatrix[location] = {}; 
    if (!regionalDemandMatrix[location][item.itemName]) regionalDemandMatrix[location][item.itemName] = {}; 
     
    // Aggregate volume by week per location 
    regionalDemandMatrix[location][item.itemName][weekStr] =  
      (regionalDemandMatrix[location][item.itemName][weekStr] || 0) + item.orderQuantity; 
  }); 
}); 
 
console.log(JSON.stringify(regionalDemandMatrix, null, 2)); 
/* 
Output reveals hyper-localized trend data: 
{ 
  "FACILITY-EAST-01": { 
    "Eco-Bottle 500ml": { "2026-05-18": 50 }, 
    "Bamboo Straws": { "2026-05-18": 100 } 
  }, 
  "FACILITY-WEST-02": { 
    "Eco-Bottle 500ml": { "2026-05-18": 30 } 
  } 
} 
*/ 

### 🤖 Predictive Superpowers for AI Agents 

With this header data available, our developer partners' AI agents can handle incredibly smart autonomous operations

**Smart Rebalancing**: If the AI agent detects a 30% demand drop in "FACILITY-EAST-01" but a 40% surge in "FACILITY-WEST-02" for the upcoming shipWeekStartDate, it can automatically issue inventory transfer requests. 

**Carrier Scheduling**: By projecting future shipping volumes (orderTransportationStatus) against scheduledDayOfWeek, the app can predict which days will experience logistics bottlenecks and pre-book freight trucks weeks in advance. 

--- 
