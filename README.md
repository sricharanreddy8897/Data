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
        Dealer dealer1 = new Dealer(101, leads1, 2024, 5);
        dealers.add(dealer1);

        List<Lead> leads2 = new ArrayList<>();
        leads2.add(new Lead(3, "Cars.com", 77777.77, 77777.77));
        Dealer dealer2 = new Dealer(102, leads2, 2024, 5);
        dealers.add(dealer2);
    }

    public Dealer getDealerWithLeads(int dealerId, int year, int month, int offset, int limit) {
        for (Dealer dealer : dealers) {
            if (dealer.getDealerId() == dealerId && dealer.getYear() == year && dealer.getMonth() == month) {
                List<Lead> leads = paginateLeads(dealer.getLeads(), offset, limit);
                dealer.setLeads(leads); // Update dealer with paginated leads
                return dealer;
            }
        }
        return null; // Return null if dealer not found with the specified criteria
    }

    private List<Lead> paginateLeads(List<Lead> allLeads, int offset, int limit) {
        int startIndex = Math.min(offset, allLeads.size());
        int endIndex = Math.min(offset + limit, allLeads.size());
        if (startIndex >= endIndex || offset < 0 || limit <= 0) {
            return new ArrayList<>(); // Return empty list for invalid pagination parameters
        }
        return allLeads.subList(startIndex, endIndex);
    }
}

import org.springframework.beans.factory.annotation.Autowired;
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

    @GetMapping("/dealerWithLeads")
    public ResponseEntity<Dealer> getDealerWithLeads(
            @RequestParam int dealerId,
            @RequestParam int year,
            @RequestParam int month,
            @RequestParam(defaultValue = "0") int offset,
            @RequestParam(defaultValue = "10") int limit) {

        Dealer dealer = mockService.getDealerWithLeads(dealerId, year, month, offset, limit);

        if (dealer != null) {
            return ResponseEntity.ok(dealer);
        } else {
            return ResponseEntity.notFound().build();
        }
    }
}



