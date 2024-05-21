import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.mockito.*;

import java.util.ArrayList;
import java.util.List;

public class DealerServiceTest {

    @InjectMocks
    private DealerService dealerService;

    @Mock
    private DealerCostAndGross dealerCostAndGross;

    @Mock
    private DealerLeadSourceCostAndGrossEntityRepository repository;

    @BeforeEach
    public void setUp() {
        MockitoAnnotations.openMocks(this);
    }

    @Test
    public void testProcessDealerData_InvalidDealerCostAndGross() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> 
            dealerService.processDealerData(1, null)
        );
        assertTrue(exception.getMessage().contains("DealerCostAndGross and its LeadSources cannot be null or empty"));
    }

    @Test
    public void testProcessDealerData_InvalidDealerId() {
        when(dealerCostAndGross.getLeadSources()).thenReturn(new ArrayList<>());
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> 
            dealerService.processDealerData(-1, dealerCostAndGross)
        );
        assertTrue(exception.getMessage().contains("Dealer ID must be a positive integer"));
    }

    @Test
    public void testProcessDealerData_NegativeCostInLead() {
        List<Lead> leads = new ArrayList<>();
        leads.add(new Lead(1, "Source", -50.0, 100.0, "January", 2022));
        when(dealerCostAndGross.getLeadSources()).thenReturn(leads);
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> 
            dealerService.processDealerData(1, dealerCostAndGross)
        );
        assertTrue(exception.getMessage().contains("Cost cannot be negative"));
    }

    @Test
    public void testProcessDealerData_DuplicateLeadSourceId() {
        List<Lead> leads = new ArrayList<>();
        leads.add(new Lead(1, "Source A", 50.0, 100.0, "January", 2022));
        leads.add(new Lead(1, "Source B", 75.0, 150.0, "January", 2022));
        when(dealerCostAndGross.getLeadSources()).thenReturn(leads);
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> 
            dealerService.processDealerData(1, dealerCostAndGross)
        );
        assertTrue(exception.getMessage().contains("Duplicate LeadSource ID found"));
    }

    // Additional test cases as needed for other validations
}
