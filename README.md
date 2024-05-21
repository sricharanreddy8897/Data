@Test
public void testProcessDealerData_Exception() {
    int dealerId = 1;
    DealerCostAndGross mockCostAndGross = mock(DealerCostAndGross.class);
    
    // Assuming the RuntimeException should indeed result in an INTERNAL_SERVER_ERROR
    doThrow(new RuntimeException("Unexpected error")).when(mockCostAndGross).getLeadSources();

    // Execute
    ResponseEntity<String> response = dealerService.processDealerData(dealerId, mockCostAndGross);

    // Verify
    assertEquals(HttpStatus.INTERNAL_SERVER_ERROR, response.getStatusCode());
}
