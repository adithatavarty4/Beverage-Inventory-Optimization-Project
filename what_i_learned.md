1) SKU --> Stock Keeping Unit 
    a) A unique code that a business assigned to each specific product it sells, to track its inventory
    b) Each variation of a product gets its own SKU. (a small blue t-shirt and a large blue t-shirt will get unique SKU)
2) Core tradeoff in Inventory Management: You always have two costs;
    a) Ordering Cost --> The cost of placing and receiving an order 
        i) It increases as you order more frequently 
        i) It decreases if you order larger quantities
        i) So each time you place an order, you have a fixed cost even if its a small or big order. 
    b) Holding Cost --> The cost of keeping inventory sitting around
        i) If buying in huge batches, many times the inventory will not sell fast enough, so the cost increases here. 
        i) Cost of Capital --> money tied up in inventory is not earning return eleswhere. Spoilage, Insurance, Storage are also part of these costs. 

    c) You want to find a space which minimizes the sum of ordering cost + holding cost. 
3) EOQ (Economic Order Quantity)
    a) How much should I order each time to minimize the total cost. 
    b) EOQ = sqrt(2DS/H) --> D = Annual Demand, S = fixed cost per order, H = annual holding cost per year
    c) Intuition: If ordering is expensive relative to holding, order bigger and in less intervals. If holding is expensive, order smaller and more often/ EOQ is the quantity where thse two costs cost and total cost is lowest. 
4) Reorder Point 
    a) At what inventory level do I need to place a new order
    b) basic formula: ROP = d * L; d = average daily demand , L = lead time (days between placing an order and receiving it)
    c) covers demand during the time you're waiting for the new order to arrive
5) Safety Stock
    a) How much extra buffer do I need in case demand or lead time is not perfectly predictable. 
    b) Demand and lead time can fluctuate as a distributor may sell more than expected or a shipment may get delayed so safety stock protects against stockouts from that variability. 
    c) SS = Z * sigma-d x sqrt(L)
    d) So then reorder point becomes d x L + SS
6) Time Series 
    a) data collected over time. How things change over time. (order matters)
    b) What people look at in time series data 
        i) Trend: Is it generally going up or down during the long run (like sales growth over year)
        ii) Seasonality: Does it repeat in a predictable cycle 
        i) Used with Demand Forecasting to check past demand to estimate the average demand which we use for ROP. Also seasonality trends tell how much more safety stock is needed b/c demand will be high at that time. 
    c) If demand is trending up, then you would want to project forward instead of copying last year demand. If its seasonality based, you could have multiple EOQs. 
    d) Moving Average --> A way to smooth out noisy data by averaging a fixed number of recent periods, and sliding the window as new data comes in. 
        i) hard to tell if theres a lot of noise if you pick only a couple of days so ts better to do a moving average to see trends more clealry. You need to keep the average moving or else you could get outdated results if demand does change. 
        i) Since you need a good estimate for average demand, you cannot choose too many days like a year because it won't catch trends. SHort window would be noisy nd react fast to the recent changes. 
        i) Since a day 3 days ago and yesterday count as the same weight, a weighted moving average is used to give emphasis on the more recent days and less weight to the older days. If demand is shifting faster, the weighted average reacts faster because the more recent data has more to say
    e) Exponential Smoothing
        i)Similar concept to weighted moving average but much clerner instead of manually assigning weights. 
        i) New Forecast = alpha * today's actual + (1-alpha) * yesterday's forecast: alpha is the number between 0 and 1 that controls how much you trust today;s number vs your past forecast. higher alpha means it weights the recent changes and lower alpha is a more smooth but slow to catch changes. 
    f) ARIMA
        i) AR ( AutoRegressive) --> tody's value depends on the past value 
        ii) I (Integrated) --> takes the difference between consecutive values to remove the trend, so data becomes stabke. It looks at the changes between each of the inputs. 
        iii) MA (Moving Average) --> today's value depends on past forecast errors (it checks how wrong the predictions of the past were)
    g) SARIMAX --> Model helps if the sales are seasonal as it just doesn't check short term memory, but long term about what happened the previous year. X adds the exgeneous variables. 
        i) Seasonal Period is how many time steps it takes for one full repeating cycle to complete. If you have yearly seasonality and your steps are everyday, your seasonal period is 365 days. 
    h) Non-Seasonal Order - This controls the short-term, non seasonal pattern in the data. This is covered with plain ARIMA
        i) (p,d,q) --> p = AR term (how many past values does the model look at to predict today) so if p = 1, that means that today's demand will be predicated based on yesterday. d = differencing (I in ARIMA). How many times do you subtract "yesterdy from today" to remove trend. q = MA term (moving average) so how many past prediction errors should the model use to correct itself. 
    

