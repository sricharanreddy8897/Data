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

    public List<Lead> getLeadsByDealerId(int dealerId, int offset, int limit) {
        for (Dealer dealer : dealers) {
            if (dealer.getDealerId() == dealerId) {
                List<Lead> allLeads = dealer.getLeads();
                return paginateLeads(allLeads, offset, limit);
            }
        }
        return new ArrayList<>(); // Return empty list if dealerId not found
    }

    private List<Lead> paginateLeads(List<Lead> allLeads, int offset, int limit) {
        int startIndex = Math.min(offset, allLeads.size());
        int endIndex = Math.min(offset + limit, allLeads.size());
        if (startIndex >= endIndex || offset < 0 || limit <= 0) {
            return new ArrayList<>(); // Return empty list for invalid pagination parameters
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
    public ResponseEntity<List<Lead>> getLeadsByDealerId(
            @RequestParam int dealerId,
            @RequestParam(defaultValue = "0") int offset,
            @RequestParam(defaultValue = "10") int limit) {

        List<Lead> leads = mockService.getLeadsByDealerId(dealerId, offset, limit);

        if (!leads.isEmpty()) {
            return ResponseEntity.ok(leads);
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}

        
        return allLeads.subList(startIndex, endIndex);
    }
}


