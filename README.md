import org.springframework.stereotype.Service;
import java.util.ArrayList;
import java.util.List;

@Service
public class MockService {

    private List<Dealer> dealers = new ArrayList<>();

    public MockService() {
        // Initialize mock data for dealers with leads
        List<Lead> leads1 = new ArrayList<>();
        leads1.add(new Lead(1, "Car Gurus", 99999.99, 99999.99));
        leads1.add(new Lead(2, "AutoTrader", 89999.99, 89999.99));
        Dealer dealer1 = new Dealer(101, leads1);
        dealers.add(dealer1);

        List<Lead> leads2 = new ArrayList<>();
        leads2.add(new Lead(3, "Cars.com", 77777.77, 77777.77));
        Dealer dealer2 = new Dealer(102, leads2);
        dealers.add(dealer2);
    }

    public List<Lead> getLeadsByDealerId(int dealerId, int pageSize, int currentPage) {
        for (Dealer dealer : dealers) {
            if (dealer.getDealerId() == dealerId) {
                List<Lead> allLeads = dealer.getLeads();
                return paginateLeads(allLeads, pageSize, currentPage);
            }
        }
        return new ArrayList<>(); // Return empty list if dealerId not found
    }

    private List<Lead> paginateLeads(List<Lead> allLeads, int pageSize, int currentPage) {
        int startIdx = (currentPage - 1) * pageSize;
        int endIdx = Math.min(startIdx + pageSize, allLeads.size());
        if (startIdx >= endIdx || startIdx < 0 || currentPage < 1) {
            return new ArrayList<>(); // Return empty list for invalid pagination parameters
        }
        return allLeads.subList(startIdx, endIdx);
    }
}


import org.springframework.beans.factory.annotation.Autowired;
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

    @GetMapping("/leadsByDealerId")
    public ResponseEntity<ResponseData> getLeadsByDealerId(
            @RequestParam int dealerId,
            @RequestParam(defaultValue = "10") int pageSize,
            @RequestParam(defaultValue = "1") int currentPage) {

        List<Lead> leads = mockService.getLeadsByDealerId(dealerId, pageSize, currentPage);

        if (!leads.isEmpty()) {
            Pagination pagination = new Pagination(leads.size(), pageSize, currentPage);
            ResponseData responseData = new ResponseData(leads, pagination);
            return ResponseEntity.ok(responseData);
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}


