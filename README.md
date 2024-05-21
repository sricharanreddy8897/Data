import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;
import java.util.Arrays;

public class DealerServiceTest {

    private DealerService dealerService = new DealerService();

    @Test
    public void testValidateInput_DealerIdNegative() {
        DealerCostAndGross dealerCostAndGross = new DealerCostAndGross();
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            dealerService.validateInput(-1, dealerCostAndGross);
        });
        assertEquals("Dealer ID must be a positive integer", exception.getMessage());
    }

    @Test
    public void testValidateInput_LeadSourceIdNegative() {
        Lead lead = new Lead(-1, 100, 200); // Assuming Lead constructor is Lead(int id, int cost, int gross)
        DealerCostAndGross dealerCostAndGross = new DealerCostAndGross(Arrays.asList(lead));

        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            dealerService.validateInput(1, dealerCostAndGross);
        });
        assertEquals("LeadSource ID must be a positive integer", exception.getMessage());
    }

    @Test
    public void testValidateInput_CostNegative() {
        Lead lead = new Lead(1, -100, 200); // Assuming Lead constructor is Lead(int id, int cost, int gross)
        DealerCostAndGross dealerCostAndGross = new DealerCostAndGross(Arrays.asList(lead));

        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            dealerService.validateInput(1, dealerCostAndGross);
        });
        assertEquals("Cost cannot be negative", exception.getMessage());
    }

    @Test
    public void testValidateInput_GrossNegative() {
        Lead lead = new Lead(1, 100, -200); // Assuming Lead constructor is Lead(int id, int cost, int gross)
        DealerCostAndGross dealerCostAndGross = new DealerCostAndGross(Arrays.asList(lead));

        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            dealerService.validateInput(1, dealerCostAndGross);
        });
        assertEquals("Gross cannot be negative", exception.getMessage());
    }

    @Test
    public void testValidateInput_DuplicateLeadSourceId() {
        Lead lead1 = new Lead(1, 100, 200);
        Lead lead2 = new Lead(1, 150, 250); // Duplicate ID
        DealerCostAndGross dealerCostAndGross = new DealerCostAndGross(Arrays.asList(lead1, lead2));

        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            dealerService.validateInput(1, dealerCostAndGross);
        });
        assertTrue(exception.getMessage().contains("Duplicate LeadSource ID found: 1"));
    }

    @Test
    public void testValidateInput_ValidInput() {
        Lead lead1 = new Lead(1, 100, 200);
        Lead lead2 = new Lead(2, 150, 250);
        DealerCostAndGross dealerCostAndGross = new DealerCostAndGross(Arrays.asList(lead1, lead2));

        assertDoesNotThrow(() -> {
            dealerService.validateInput(1, dealerCostAndGross);
        });
    }
}
