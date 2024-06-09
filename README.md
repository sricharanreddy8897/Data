
1. Controller Class
java
Copy code
package com.example.bff.controller;

import com.example.bff.service.DealerService;
import com.example.bff.model.DealerCostAndGross;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/v1")
public class DealerController {

    private final DealerService dealerService;

    @Autowired
    public DealerController(DealerService dealerService) {
        this.dealerService = dealerService;
    }

    @PostMapping("/dealers/{dealerId}/lead-sources/import-cost-gross-batch")
    public ResponseEntity<String> saveCostAndGrossForLeads(
            @RequestBody DealerCostAndGross dealerRequest,
            @PathVariable("dealerId") int dealerId) {
        return dealerService.processDealerData(dealerId, dealerRequest);
    }
}
2. Service Class
java
Copy code
package com.example.bff.service;

import com.example.bff.model.DealerCostAndGross;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Service;
import org.springframework.web.client.HttpClientErrorException;
import org.springframework.web.client.HttpServerErrorException;
import org.springframework.web.client.RestTemplate;

@Service
public class DealerService {

    private final RestTemplate restTemplate;
    private final String backendUrl = "http://your-backend-service-url"; // Replace with your actual backend URL

    @Autowired
    public DealerService(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    public ResponseEntity<String> processDealerData(int dealerId, DealerCostAndGross dealerRequest) {
        String backendEndpoint = backendUrl + "/dealerId/marketing/lead-sources/import-cost-gross-batch";

        try {
            ResponseEntity<String> response = restTemplate.postForEntity(
                    backendEndpoint,
                    dealerRequest,
                    String.class,
                    dealerId
            );

            return new ResponseEntity<>(response.getBody(), response.getStatusCode());
        } catch (HttpClientErrorException | HttpServerErrorException e) {
            return ResponseEntity.status(e.getStatusCode()).body(e.getResponseBodyAsString());
        } catch (Exception e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("An error occurred while processing the request.");
        }
    }
}
3. Model Class
java
Copy code
package com.example.bff.model;

import java.util.List;

public class DealerCostAndGross {
    private List<LeadSource> leadSources;

    // Getters and Setters

    public List<LeadSource> getLeadSources() {
        return leadSources;
    }

    public void setLeadSources(List<LeadSource> leadSources) {
        this.leadSources = leadSources;
    }

    public static class LeadSource {
        private int leadSourceId;
        private double cost;
        private double gross;

        // Getters and Setters

        public int getLeadSourceId() {
            return leadSourceId;
        }

        public void setLeadSourceId(int leadSourceId) {
            this.leadSourceId = leadSourceId;
        }

        public double getCost() {
            return cost;
        }

        public void setCost(double cost) {
            this.cost = cost;
        }

        public double getGross() {
            return gross;
        }

        public void setGross(double gross) {
            this.gross = gross;
        }
    }
}
4. RestTemplate Configuration
If not already configured, you need to configure RestTemplate as a bean in your Spring application.

java
Copy code
package com.example.bff.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class AppConfig {

    @Bean
    public RestTemplate restTemplate() {
        return new RestTemplate();
    }
}
5. Application Entry Point
Make sure you have your main application class to start the Spring Boot application.

java
Copy code
package com.example.bff;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class BffApplication {

    public static void main(String[] args) {
        SpringApplication.run(BffApplication.class, args);
    }
}
Folder Structure
css
Copy code
src/main/java/com/example/bff/
    ├── BffApplication.java
    ├── config/
    │   └── AppConfig.java
    ├── controller/
    │   └── DealerController.java
    ├── model/
    │   └── DealerCostAndGross.java
    └── service/
        └── DealerService.java
Dependencies
Make sure your pom.xml includes the necessary dependencies:

xml
Copy code
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
Summary
This structure ensures that your controller, service, and model classes are properly separated and organized, following the standard practices of a Spring Boot application. This makes the code more maintainable and easier to manage.

package com.example.bff.service.impl;

import com.example.bff.model.DealerCostAndGross;
import com.example.bff.service.DealerService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpMethod;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Service;
import org.springframework.web.client.HttpClientErrorException;
import org.springframework.web.client.HttpServerErrorException;
import org.springframework.web.client.RestTemplate;

@Service
public class DealerServiceImpl implements DealerService {

    private final RestTemplate restTemplate;
    private final String backendUrl = "http://your-backend-service-url"; // Replace with your actual backend URL

    @Autowired
    public DealerServiceImpl(RestTemplate restTemplate) {
        this.restTemplate = restTemplate;
    }

    @Override
    public ResponseEntity<String> processDealerData(int dealerId, DealerCostAndGross dealerRequest) {
        String backendEndpoint = backendUrl + "/dealerId/marketing/lead-sources/import-cost-gross-batch";
        
        try {
            // Create the HTTP entity with the request body
            HttpEntity<DealerCostAndGross> entity = new HttpEntity<>(dealerRequest);
            
            // Make the POST request using RestTemplate
            ResponseEntity<String> response = restTemplate.exchange(
                    backendEndpoint,
                    HttpMethod.POST,
                    entity,
                    String.class,
                    dealerId
            );

            // Return the response from the backend service
            return new ResponseEntity<>(response.getBody(), response.getStatusCode());
        } catch (HttpClientErrorException | HttpServerErrorException e) {
            // Return the error response from the backend service
            return ResponseEntity.status(e.getStatusCode()).body(e.getResponseBodyAsString());
        } catch (Exception e) {
            // Return a generic internal server error response
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body("An error occurred while processing the request.");
        }
    }
}



