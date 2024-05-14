import org.springframework.stereotype.Service;
import java.util.ArrayList;
import java.util.List;

@Service
public class MockService {

    public List<ResponseData> getMockResponses(List<Long> dealerIds, int month, int year, int offset, int limit) {
        List<ResponseData> responseDataList = new ArrayList<>();

        // Generate mock response data for each dealerId
        for (Long dealerId : dealerIds) {
            // Create mock lead source data
            LeadSource leadSource = new LeadSource(1, "Car Gurus", 99999.99, 99999.99);

            // Create mock pagination data
            Pagination pagination = new Pagination(1, 1, 1);

            // Create ResponseData object and add to the list
            ResponseData responseData = new ResponseData(Collections.singletonList(leadSource), pagination);
            responseDataList.add(responseData);
        }

        return responseDataList;
    }
}


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;
import java.util.List;

@RestController
public class MockController {

    private final MockService mockService;

    @Autowired
    public MockController(MockService mockService) {
        this.mockService = mockService;
    }

    @GetMapping("/mockEndpoint")
    public ResponseEntity<List<ResponseData>> getMockData(
            @RequestParam List<Long> dealerIds,
            @RequestParam int month,
            @RequestParam int year,
            @RequestParam int offset,
            @RequestParam int limit
    ) {
        // Call the service to get the list of mock response data
        List<ResponseData> mockResponses = mockService.getMockResponses(dealerIds, month, year, offset, limit);

        // Return the list of mock responses with HTTP status 200 (OK)
        return ResponseEntity.ok(mockResponses);
    }
}

