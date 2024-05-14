import org.springframework.stereotype.Service;
import java.util.ArrayList;
import java.util.List;

@Service
public class MockService {

    // Simulated list of dealers with leads
    private List<Dealer> dealers = new ArrayList<>();

    public MockService() {
        // Mock data for Dealer 1
        List<Lead> leads1 = new ArrayList<>();
        leads1.add(new Lead(1, "Car Gurus", 99999.99, 99999.99));
        leads1.add(new Lead(2, "AutoTrader", 89999.99, 89999.99));
        Dealer dealer1 = new Dealer(101, leads1);
        dealers.add(dealer1);

        // Mock data for Dealer 2
        List<Lead> leads2 = new ArrayList<>();
        leads2.add(new Lead(3, "Cars.com", 77777.77, 77777.77));
        Dealer dealer2 = new Dealer(102, leads2);
        dealers.add(dealer2);
    }

    public List<Lead> getLeadsByDealerId(int dealerId) {
        for (Dealer dealer : dealers) {
            if (dealer.getDealerId() == dealerId) {
                return dealer.getLeads();
            }
        }
        return new ArrayList<>(); // Return empty list if dealerId not found
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
    public ResponseEntity<List<Lead>> getLeadsByDealerId(@RequestParam int dealerId) {
        // Call the mock service to retrieve leads for the specified dealerId
        List<Lead> leads = mockService.getLeadsByDealerId(dealerId);

        if (!leads.isEmpty()) {
            // Return the list of leads with HTTP status 200 (OK) if leads found
            return ResponseEntity.ok(leads);
        } else {
            // Return HTTP status 404 (Not Found) if no leads found for the specified dealerId
            return ResponseEntity.notFound().build();
        }
    }
}



