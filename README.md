@Test
public void whenDealerIdIsNonPositive_thenThrowException() {
    DealerCostAndGross mockCostAndGross = new DealerCostAndGross(); // Assuming you have a no-arg constructor
    Exception exception = assertThrows(IllegalArgumentException.class, () -> {
        dealerService.processDealerData(0, mockCostAndGross); // Using zero to test boundary condition
    });

    assertTrue(exception.getMessage().contains("Dealer ID must be a positive integer"));
}



@Test
public void whenCostIsNegative_thenThrowException() {
    DealerCostAndGross mockCostAndGross = new DealerCostAndGross();
    Lead lead = new Lead(); // Assuming you have setters to set these values
    lead.setCost(-1.0);
    mockCostAndGross.setLeadSources(Arrays.asList(lead));
    
    Exception exception = assertThrows(IllegalArgumentException.class, () -> {
        dealerService.processDealerData(1, mockCostAndGross);
    });

    assertTrue(exception.getMessage().contains("Cost cannot be negative"));
}

@Test
public void whenGrossIsNegative_thenThrowException() {
    DealerCostAndGross mockCostAndGross = new DealerCostAndGross();
    Lead lead = new Lead();
    lead.setGross(-1.0);
    mockCostAndGross.setLeadSources(Arrays.asList(lead));
    
    Exception exception = assertThrows(IllegalArgumentException.class, () -> {
        dealerService.processDealerData(1, mockCostAndGross);
    });

    assertTrue(exception.getMessage().contains("Gross cannot be negative"));
}


@Test
public void whenDuplicateLeadSourceIds_thenThrowException() {
    DealerCostAndGross mockCostAndGross = new DealerCostAndGross();
    Lead lead1 = new Lead();
    lead1.setLeadSourceId(1);
    Lead lead2 = new Lead();
    lead2.setLeadSourceId(1); // Duplicate ID
    mockCostAndGross.setLeadSources(Arrays.asList(lead1, lead2));
    
    Exception exception = assertThrows(IllegalArgumentException.class, () -> {
        dealerService.processDealerData(1, mockCostAndGross);
    });

    assertTrue(exception.getMessage().contains("Duplicate LeadSource ID found"));
}



