typedef long long int ll;

class Solution {
    
    bool IsPossible (ll min_power, const vector<ll>& default_powers, int extra_stations, int r, int num_stations) {
        vector<ll> extra_power (num_stations+1);
        
        for (int j = 0; j < num_stations; j ++) {
            extra_power[j] += (j > 0? extra_power[j-1] : 0);
            
            ll cur_power = default_powers[j] + extra_power[j];
            ll required = max (0LL, min_power - cur_power);
            if (required == 0) continue;
            
            if (required > extra_stations) return false;
            extra_stations -= required;
            
            extra_power[j] += required;
            extra_power[min(num_stations, j+2*r+1)] -= required;
        }
        
        return true;
    }
    
public:
    long long maxPower(vector<int>& stations, int range, int k) {
        int n = stations.size();
     
        vector<ll> station_powers(n+1);
        for (int j = 0; j < n; j ++) {
            station_powers[max(0, j-range)] += stations[j];
            station_powers[min(n, j+range+1)] -= stations[j];
        }
        for (int j = 1; j <= n; j ++) station_powers[j] += station_powers[j-1];
        
        ll l = 0, r = 1e11;
        while (l < r) {
            ll m = (l+r) >> 1;
            
            if (IsPossible (m+1, station_powers, k, range, n)) l = m+1;
            else r = m;
        }
        
        return l;
    }
};