import java.util.ArrayList;
import java.util.List;

public class MockResponse {

    public static class LeadSource {
        private int leadSourceId;
        private String sourceName;
        private double cost;
        private double gross;

        public LeadSource(int leadSourceId, String sourceName, double cost, double gross) {
            this.leadSourceId = leadSourceId;
            this.sourceName = sourceName;
            this.cost = cost;
            this.gross = gross;
        }

        public int getLeadSourceId() {
            return leadSourceId;
        }

        public String getSourceName() {
            return sourceName;
        }

        public double getCost() {
            return cost;
        }

        public double getGross() {
            return gross;
        }
    }

    public static class ResponseData {
        private List<LeadSource> data;
        private Pagination pagination;

        public ResponseData(List<LeadSource> data, Pagination pagination) {
            this.data = data;
            this.pagination = pagination;
        }

        public List<LeadSource> getData() {
            return data;
        }

        public Pagination getPagination() {
            return pagination;
        }
    }

    public static class Pagination {
        private int totalRecords;
        private int pageSize;
        private int currentPage;

        public Pagination(int totalRecords, int pageSize, int currentPage) {
            this.totalRecords = totalRecords;
            this.pageSize = pageSize;
            this.currentPage = currentPage;
        }

        public int getTotalRecords() {
            return totalRecords;
        }

        public int getPageSize() {
            return pageSize;
        }

        public int getCurrentPage() {
            return currentPage;
        }
    }

    public static void main(String[] args) {
        // Create mock lead source data
        List<LeadSource> leadSources = new ArrayList<>();
        leadSources.add(new LeadSource(1, "Car Gurus", 99999.99, 99999.99));

        // Create mock pagination data
        Pagination pagination = new Pagination(1, 1, 1);

        // Create mock response data
        ResponseData responseData = new ResponseData(leadSources, pagination);

        // Print the mock response in JSON format
        System.out.println(convertToJson(responseData));
    }

    // Method to convert Java object to JSON string (you can use a library like Gson or Jackson for this)
    private static String convertToJson(ResponseData responseData) {
        // Sample JSON conversion (using a JSON library like Gson or Jackson)
        // For simplicity, we can use a manual JSON format here
        StringBuilder jsonBuilder = new StringBuilder();
        jsonBuilder.append("{\n");
        jsonBuilder.append("  \"data\": ").append(convertLeadSourcesToJson(responseData.getData())).append(",\n");
        jsonBuilder.append("  \"pagination\": ").append(convertPaginationToJson(responseData.getPagination())).append("\n");
        jsonBuilder.append("}");
        return jsonBuilder.toString();
    }

    // Method to convert list of LeadSource objects to JSON array
    private static String convertLeadSourcesToJson(List<LeadSource> leadSources) 





    import org.springframework.stereotype.Service;

import java.util.Collections;

@Service
public class MockService {

    public ResponseData getMockResponse(Long dealerId, int month, int year, int offset, int limit) {
        // Create mock lead source data
        LeadSource leadSource = new LeadSource(1, "Car Gurus", 99999.99, 99999.99);

        // Create mock pagination data
        Pagination pagination = new Pagination(1, 1, 1);

        // Create mock response data
        ResponseData responseData = new ResponseData(Collections.singletonList(leadSource), pagination);

        return responseData;
    }
}

import org.springframework.stereotype.Service;

import java.util.Collections;

@Service
public class MockService {

    public ResponseData getMockResponse(Long dealerId, int month, int year, int offset, int limit) {
        // Create mock lead source data
        LeadSource leadSource = new LeadSource(1, "Car Gurus", 99999.99, 99999.99);

        // Create mock pagination data
        Pagination pagination = new Pagination(1, 1, 1);

        // Create mock response data
        ResponseData responseData = new ResponseData(Collections.singletonList(leadSource), pagination);

        return responseData;
    }
}





import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MockController {

    private final MockService mockService;

    @Autowired
    public MockController(MockService mockService) {
        this.mockService = mockService;
    }

    @GetMapping("/mockEndpoint")
    public ResponseEntity<ResponseData> getMockData(
            @RequestParam Long dealerId,
            @RequestParam int month,
            @RequestParam int year,
            @RequestParam int offset,
            @RequestParam int limit
    ) {
        // Call the service to get the mock response data
        ResponseData mockResponse = mockService.getMockResponse(dealerId, month, year, offset, limit);

        // Return the mock response with HTTP status 200 (OK)
        return ResponseEntity.ok(mockResponse);
    }
}


import org.springframework.stereotype.Service;

import java.util.Collections;
import java.util.List;

@Service
public class MockService {

    public ResponseData getMockResponse(Long dealerId, int month, int year, int offset, int limit) {
        // Create mock lead source data
        LeadSource leadSource = new LeadSource(1, "Car Gurus", 99999.99, 99999.99);

        // Create mock pagination data
        Pagination pagination = new Pagination(1, 1, 1);

        // Create mock response data
        List<LeadSource> leadSources = Collections.singletonList(leadSource);
        return new ResponseData(leadSources, pagination);
    }
}


