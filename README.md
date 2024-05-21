import static org.junit.Assert.*;
import static org.mockito.Mockito.*;

import org.junit.Before;
import org.junit.Test;
import org.mockito.*;

public class DealerServiceTest {

    @InjectMocks
    private DealerService dealerService;

    @Mock
    private DealerCostAndGross dealerCostAndGross;

    @Mock
    private DealerLeadSourceCostAndGrossEntityRepository repository;

    @Before
    public void setUp() {
        MockitoAnnotations.initMocks(this);
    }

    @Test(expected = IllegalArgumentException.class)
    public void testProcessDealerData_InvalidDealerCostAndGross() {
        dealerService.processDealerData(1, null);
    }

    @Test(expected = IllegalArgumentException.class)
    public void testProcessDealerData_InvalidDealerId() {
        when(dealerCostAndGross.getLeadSources()).thenReturn(new ArrayList<>());
        dealerService.processDealerData(-1, dealerCostAndGross);
    }

    @Test(expected = IllegalArgumentException.class)
    public void testProcessDealerData_NegativeCostInLead() {
        List<Lead> leads = new ArrayList<>();
        leads.add(new Lead(1, "Source", -50.0, 100.0, "January", 2022));
        when(dealerCostAndGross.getLeadSources()).thenReturn(leads);
        dealerService.processDealerData(1, dealerCostAndGross);
    }

    @Test(expected = IllegalArgumentException.class)
    public void testProcessDealerData_DuplicateLeadSourceId() {
        List<Lead> leads = new ArrayList<>();
        leads.add(new Lead(1, "Source A", 50.0, 100.0, "January", 2022));
        leads.add(new Lead(1, "Source B", 75.0, 150.0, "January", 2022));
        when(dealerCostAndGross.getLeadSources()).thenReturn(leads);
        dealerService.processDealerData(1, dealerCostAndGross);
    }

    // Additional test cases as needed for other validations
}
