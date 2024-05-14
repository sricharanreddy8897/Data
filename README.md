import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;

@Service
public class DealerService {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    public DealerService(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public String processDealerData(DealerDTO dealerDTO) {
        try {
            List<Object[]> batchArgs = new ArrayList<>();

            for (LeadDTO leadDTO : dealerDTO.getLeads()) {
                Object[] args = {
                        leadDTO.getLeadSourceId(),
                        leadDTO.getSourceName(),
                        leadDTO.getCost(),
                        leadDTO.getGross(),
                        dealerDTO.getDealerId(),
                        dealerDTO.getMonth() + "-" + dealerDTO.getYear() // Assuming reporting month format is MM-yyyy
                };
                batchArgs.add(args);
            }

            String sql = "INSERT INTO cost_and_gross_details (Lead_source_id, Source_name, Cost, Gross, Dealer_id, Reporting_month) VALUES (?, ?, ?, ?, ?, ?)";
            jdbcTemplate.batchUpdate(sql, batchArgs);

            return "Data inserted successfully";
        } catch (Exception e) {
            // Handle exception, log error, etc.
            return "Failed to insert data: " + e.getMessage();
        }
    }
}
