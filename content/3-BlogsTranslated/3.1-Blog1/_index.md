---
title: "Blog 1 - Riskthinking.AI Climate Earth Digital Twin on AWS"
date: 2025-04-02
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# Introducing Riskthinking.AI Climate Earth Digital Twin on AWS

**Authors**: Ilan Gleiser, Andrew Wiebe, Dilshan Kathriarachchi, Marco Masciola, and Ron Dembo  
**Published**: April 2, 2025  
**Categories**: Amazon Elastic Kubernetes Service, AWS Batch, High Performance Computing, Partner solutions

---

## Overview

Climate change is an escalating global challenge, with rising temperatures, shifting weather patterns, and more frequent extreme events already impacting ecosystems, economies, and infrastructure. Public and private assets are increasingly exposed to these risks.

### Power Sector Challenges

As far as the **power sector** is concerned, power plants, transmission lines, and distribution networks face significant challenges:

- **Prolonged heat waves** reduce the efficiency of thermal power plants and can cause grid overloads
- **Hydroelectric plants** suffer during periods of drought, as lower water levels reduce their ability to generate electricity
- **Coastal power stations** are vulnerable to rising sea levels and storm surges
- **Wind turbines and solar panels** can be damaged by increasingly severe storms

These impacts are only expected to grow in frequency and intensity, making it critical for infrastructure systems to assess and mitigate climate risks to ensure long-term resilience.

### Italy as a Case Study

Italys electricity infrastructure offers a compelling example of the urgent need to assess climate risk in the energy sector. The country relies on a diverse energy mix, including solar, wind, hydro, and fossil fuels, making it particularly exposed to a wide range of physical climate risks.

---

## Riskthinking.AIs Climate Earth Digital Twin (CDT)

To address these challenges, **riskthinking.AI** has developed the Climate Earth Digital Twin (CDT) platform to assess climate change risks and impacts. The CDT leverages Amazon Web Services (AWS) HPC infrastructure to provide the massive computational power needed to run complex climate simulations and machine learning models at a global scale.

### Database Scale

Riskthinking.AI developed the Climate Earth Digital Twin (CDT) to assess the financial risks and impacts of climate change across scales, from entire economies to individual assets. CDT integrates data on millions of physical assetssuch as power plantswith detailed climate projections like future temperature shifts and extreme weather.

CDTs database covers:
- **13,000+** public parent companies
- **5 million+** physical assets
- Each geospatially aligned and assessed for exposure to various climate risks

### Methodology

Uniquely, CDT uses **probabilistic distributions** instead of deterministic forecasts to simulate a wide range of future scenarios. Multifactor stress testing enables customers to evaluate how multiple risks, occurring simultaneously, could affect an assets value over time.

### Computational Requirements

Running such a comprehensive climate risk analysis requires **vast storage and computational power**:

- CDT processes **petabytes of data**
- Aligning climate and asset information both spatially and temporally
- Every asset assessed under different climate hazards (sea levels, extreme weather)
- Multiple future periods: **5, 10, 15, 20, and 25 years**
- Each scenario computed for **multiple interacting risks** evolving over decades

This multifactor stress testing, powered by the **patented Dembo algorithm**, requires enormous computational resources to handle the complexity of dynamic assets and hazards.

### Output Metrics

The end result of the CDT platform is a multifactor stress test that aggregates exposure to and impacts from various climate change hazards:

- **Extreme heat**
- **Floods**
- **Wildfires**
- **Tropical cyclones**

The analysis outputs:
- **Value-at-Risk (VaR)** - aggregate climate change risk metrics
- **Exposure Score** - probability of climate change impacts in the future
- Disaggregated metrics revealing which specific climate risk factor contributes to overall risk
- Applied to every grid cell on Earth at **~10 km x 10 km** spatial resolution

---

## Built with AWS

The Climate Earth Digital Twin (CDT) runs on AWS using **Amazon Elastic Kubernetes Service (EKS)**, which provides the backbone for orchestrating the large-scale climate simulations that drive the CDT (Figure 1).

![Figure 1: CDT Architecture](/images/3.1-Blog1/image11.png)
*Figure 1. The Climate Earth Digital Twin leverages AWS's High Performance Computing (HPC) capabilities, combining Amazon EKS for orchestrating large-scale simulations with AWS Batch for efficient resource allocation and scaling. This powerful HPC architecture enables the CDT to process billions of climate simulations, dynamically scaling from 100 to 1,000,000 compute instances as needed.*

### Core Architecture

By utilizing EKS, Riskthinking.AI can efficiently manage and scale containerized applications, allowing the CDT to process **more than from 300 billion to over 1 trillion simulations**. This infrastructure enables the CDT to be both flexible and reliable in its computational capacity.

New versions of the CDT are deployed regularly via **Amazon Elastic Container Registry (ECR)**, ensuring seamless updates to the application while maintaining continuity in ongoing simulations.

#### Two Core Services

1. **Climate Earth Digital Twin Application (Analytical Engine)** - where the computational simulations are executed
2. **Climate Earth Digital Twin API** - provides customers with access to the data and functionality

The Analytical Engine uses AWSs compute layer and is supported by **AWS Lambda-powered data pipelines**, enabling highly efficient, serverless compute tasks. These Lambda-powered pipelines streamline the automation of data ingestion, transformation, and processing.

#### Resource Management

This architecture ensures stability of features and availability of compute resources by combining **EKS and AWS Batch**. The Batch scheduler coordinates with EKS to allocate compute resources to specific nodes based on workload requirements. This automated resource allocation simplifies deployment of compute resources according to changing workload demandswhether 100 or 1 million compute instances are required.

### Storage and Database Services

**Amazon S3** is the CDTs primary storage layer, storing vast climate datasets required for global simulations. With S3, Riskthinking.AI benefits from virtually unlimited storage capacity and robust data durability.

**Amazon RDS (PostgreSQL)** provides a robust, managed relational database solution for storing structured data essential to the simulations, such as historical climate patterns and projections.

**Amazon ElastiCache (Redis)** acts as a caching layer to reduce latency and speed up access to frequently requested data.

---

## Simple Setup with the CDT API
![Simple Setup with the CDT API](/images/3.1-Blog1/image5.png)
The CDT API plays a crucial role in delivering the output of these simulations to customers, providing a direct interface to access the CDTs climate risk analytics.  

### Configuration Example

```python
# API key and endpoint configuration for accessing RiskThinking.AIs Climate Digital Twin
API_URL = "https://api.riskthinking.ai"

# Climate risk parameters: focus on specific time horizons and scenario pathway
HORIZONS = {2025, 2030, 2050, 2070, 2090}  # Future projection years
PATHWAYS = {"SV"}  # "SV" refers to the Stochastic View scenario developed by RiskThinking.AI
```

The API is designed to interact seamlessly with the Analytical Engine, ensuring that large datasets and simulation results are readily available and quickly accessible. By leveraging the scalable infrastructure provided by EKS and Lambda, the API delivers consistent performance, even under heavy workloads, allowing customers to access real-time climate risk insights reliably.

### Fetching Physical Assets from the CDT

The code below fetches physical assets related to the electric power generation, transmission, and distribution sector in Italy from an API.

```python
all_assets = []
cursor = ""

# Loop through paginated API responses to collect asset data
while True:
    # Fetch Electric Grid physical assets in Italy
    response = requests.get(
        f"{API_URL}/v3/assets",
        params={
            "country": "ITA",
            "asset_type": "Electric Power Generation, Transmission and Distribution",
            "limit": 100,
            "cursor": cursor,
            "key": API_KEY,
        },
    ).json()
    
    # Parse relevant asset details (id, state, coordinates, etc.)
    assets = [
        {
            "id": asset["id"],
            "state": asset["state"],  # Regional/state-level information
            "longitude": asset["longitude"],  # Geospatial data
            "latitude": asset["latitude"],
            "hex_id": asset["hex_id"],
        }
        for asset in response["results"]
    ]
    
    # Append the fetched assets to the list
    all_assets.extend(assets)
    
    # Break the loop if the last page of the response has been reached
    if response["pagination"]["last_page"]:
        break
```

### Enriching Physical Assets with CDT Climate Risk Analytics

This code enriches the previously collected physical asset data with climate risk analytics from the Climate Earth Digital Twin (CDT).

```python
# Loop through all collected assets and fetch climate risk data
for asset in all_assets:
    asset_id = asset["id"]  # Extract the unique asset ID
    
    # API request to fetch climate risk data for the current asset
    url = f"{API_URL}/v3/assets/{asset_id}/climate/risks?key={API_KEY}"
    response = requests.get(url).json()
    
    # Store relevant climate risk scores for specified horizons and scenario pathway
    all_climate_scores.extend(
        [
            {
                "id": asset_id,
                "state": asset["state"],
                "hex_id": asset["hex_id"],
                "horizon": score["horizon"],  # The future year (horizon) under consideration
                "pathway": score["pathway"],  # Scenario pathway, e.g., Stochastic View (SV)
                "expected_impact": score["expected_impact"],  # Expected climate impact on the asset
                "var_95": score["var_95"],  # Value-at-Risk (VaR) at the 95% confidence level
            }
            for score in response["results"]
            if score["pathway"] in PATHWAYS
            and score["horizon"] in HORIZONS  # Filter for the relevant scenarios and time horizons
        ]
    )
```

---

## Spatial Distribution of Italys Power Generation Facilities

Riskthinking.AIs asset database contains a wide variety of electrical power generation and distribution assets, ranging from power stations and substations, to solar farms, hydroelectric power plants, and fossil fuel electricity generation facilities, among others.

![Figure 2: Italy Power Plants Distribution](/images/3.1-Blog1/image2.png)
*Figure 2. Distribution of fossil fuel power plants, hydroelectric facilities, and solar farms across Italy, showing the geographic concentration of hydroelectric plants in northern regions.*

### Key Observations

A subset of these categories (namely fossil fuel power plants, power plants, hydroelectricity facilities, and solar farms) are depicted in Figure 2 for illustrative purposes.

**Geographic Concentration Risk**: While the solar and fossil fuel plants are distributed somewhat uniformly across the country, **hydroelectricity generation is highly concentrated in the northern regions** of the country, which poses a significant climate risk.

Climate change impacts, such as altered precipitation patterns and prolonged droughts, are often region-specific, meaning that a **single climatic event could simultaneously affect multiple dams** if they are situated along the same river basin. This geographic concentration heightens the vulnerability of the entire hydroelectric system.

---

## Climate Change Risk to Italys Power Infrastructure

Continuing the focus on the subset of electricity infrastructure depicted in Figure 2, our results show that **freeze-thaw cycles and extreme heat** currently pose the greatest threat to fossil fuel and solar power plants, while hydroelectric power generation facilities are most impacted by extreme cold and freeze-thaw cycles.

### Freeze-Thaw Impact

Freeze-thaw cycles can cause heavy damage to concrete structures like dams by causing water that infiltrates cracks to expand when frozen, exerting pressure on the surrounding material. Repeated expansion and contraction weaken and degrade materials, leading to cracks and possibly structural damage.

### Future Projections

Towards the end of this century, the impact from the various risk factors, especially **extreme heat and freeze-thaw cycles**, is projected to increase (Figure 3).

![Figure 3: Risk Pattern Evolution](/images/3.1-Blog1/image6.png)
*Figure 3. Spatial patterns of climate risk impacts comparing 2025 and 2090 projections for extreme cold, extreme heat, and freeze-thaw cycles across fossil fuel, hydroelectric, and solar power facilities in Italy.*

#### Specific Impacts

**Hydroelectric Plants**:
- Maximum impact from freeze-thaw currently: **2.48%**
- Projected by 2090: **5.27%**
- Especially worrisome since a large majority of Italys dams are located in the same region

**Solar Power Plants**:
- Solar panels perform best within **15C to 35C**
- High temperatures can reduce power output by **10-25%**
- In extreme heat, panels can reach **65C**, negatively affecting efficiency

**All Infrastructure Types**:
- Extreme heat will continue to affect all three electricity generation infrastructure types
- Very consistent pattern observed for the whole country (Figure 4)

![Figure 4: Impact by Asset Value](/images/3.1-Blog1/image7.png)
*Figure 4. Asset value impacts from climate risk factors (extreme cold, extreme heat, freeze-thaw cycles) across different electricity generation facility types, color-coded by Italian regions.*

---

## Conclusion

From our analysis, it is evident that as climate change continues to intensify, Italys power generation infrastructure faces increasing risks. Extreme weather events such as prolonged heat waves, freeze-thaw cycles, and other hazards pose significant threats to the nations energy security.

### Key Findings

The geographic concentration of hydroelectric facilities, combined with region-specific risks, makes the energy sector particularly vulnerable.

The application of Riskthinking.AIs Climate Earth Digital Twin (CDT) has enabled a comprehensive assessment of these risks, providing actionable insights for decision-makers.

### Value Proposition

Our analysis offers:
- **Aggregated metrics** such as Value-at-Risk (VaR) and climate change exposure probability
- **Disaggregated metrics** revealing which specific risk factors contribute most to overall exposure
- Probabilistic models and multifactor stress testing across emissions scenarios

### Recommendations

By using probabilistic models and multifactor stress testing across emissions scenarios, the CDT offers a powerful tool for:
- Planning future infrastructure investments
- Risk mitigation
- Climate adaptation strategies

As countries like Italy transition toward renewable energy, assessing and addressing climate risks will be essential for ensuring long-term resilience and sustainability in the power sector.

We encourage other governments and organizations to undertake similar climate risk assessments for their critical infrastructure.

---

## Contact Information

**To learn more about Climate Earth Digital Twin:**
- Email: info@riskthinking.ai
- Website: www.riskthinking.ai

**For information on leveraging AWS services for climate modeling:**
- Email: aws-hpc-info@amazon.com

---

## References

[1] Ron Dembo, *Risk Thinking, Appendix 1*, Archway Press 2020  
[2] Ron Dembo, *Algorithmic Scenario Generation*, Risk.Net, July 2020

---

## About the Authors

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image3.png" alt="Ilan Gleiser" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Ilan Gleiser</h3>
    <p>Ilan Gleiser is a Principal GenAI Specialist at AWS WWSO Frameworks team focusing on developing scalable Artificial General Intelligence architectures and optimizing foundation model training and inference. With a rich background in AI and machine learning, Ilan has published over 20 blogs and delivered 100+ prototypes globally over the last 5 years. Ilan holds a Master's degree in mathematical economics.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image1.png" alt="Andrew Wiebe" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Andrew Wiebe</h3>
    <p>Andrew Wiebe Chief Science Officer at Riskthinking.AI, is an atmospheric scientist with 20 years of experience in boundary layer meteorology, numerical weather prediction, operational forecasting, climate analysis, dispersion modeling, industrial emission inventories, statistical analysis, climate analytics and climate change research. He has expertise in high-performance computing and data analysis and has authored and co-authored numerous peer-reviewed articles. Andrew currently sits on the World Climate Research Program's Coupled Model Intercomparison Project Phase 7 (WCRP-CMIP7) Impact and Adaptation Author Team and Data Advisory Team.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image4.png" alt="Dilshan Kathriarachchi" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Dilshan Kathriarachchi</h3>
    <p>Dilshan Kathriarachchi is a seasoned technology leader with a proven track record of scaling businesses, building high-performing teams, and spearheading product innovation. Dilshan Kathriarachchi currently serves as CTO at Riskthinking.AI. Dilshan was instrumental in transforming EQ Works (TSXV: EQ) from an AdTech provider to a data-driven geospatial analytics business during his tenure as CTO. Earlier in his career, as President and CTO of Addictive Mobility, Dilshan spearheaded establishing one of Canada's first AdTech companies. With over 15 years of experience, he leverages his extensive expertise in machine learning, data infrastructure, AI and geospatial analytics to drive the company's mission of providing actionable climate risk intelligence.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image8.png" alt="Marco Masciola" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Marco Masciola</h3>
    <p>Marco Masciola is a Senior Partner Solutions Architect for the Global Sustainability Partner Segment at Amazon Web Services. Marco helps AWS partners build and deliver their sustainability solutions through the AWS cloud.</p>
  </div>
</div>

<div style="display: flex; align-items: flex-start; margin-bottom: 20px;">
  <img src="/images/3.1-Blog1/image10.png" alt="Ron Dembo" style="width: 120px; height: 120px; object-fit: cover; border-radius: 50%; margin-right: 20px;" />
  <div>
    <h3 style="margin-top: 0;">Ron Dembo</h3>
    <p>Dr. Ron Dembo has been an Associate Professor at Yale, visiting professor at MIT, consultant to Goldman Sachs and other large corporations and banks, and author. He Founded Algorithmics, which he grew to become the largest supplier of enterprise financial risk systems to banks worldwide before selling it to Fitch and then IBM. He has published many articles in scientific journals and is author of numerous worldwide patents. He was made a lifetime fellow of the Fields Institute of Mathematics in 2007 for his contribution to Canadian Mathematics and has received many awards for his work in optimization, risk management and climate change. As Founder and CEO of Riskthinking.AI, Ron has applied his patented multi-factor scenario modelling expertise to create a data platform and analytics engine for measuring and managing climate financial risk.</p>
  </div>
</div>

---

**Tags**: Weather, Climate Risk, Digital Twin, HPC, Amazon EKS, AWS Batch, Amazon S3

