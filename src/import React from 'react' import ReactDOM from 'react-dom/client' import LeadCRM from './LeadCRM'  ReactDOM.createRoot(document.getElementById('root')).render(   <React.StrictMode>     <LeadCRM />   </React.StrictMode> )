import { useState, useEffect, useMemo } from "react";

// ─── MOCK DATA (replace with Supabase) ───────────────────────────────────────
const MOCK_LEADS = [
  {
    id: 1, sno: 1, date: "2025-05-01", business_name: "Al-Noor Traders", category: "Retail",
    address: "Shop 12, Main Bazaar", area: "Saddar", state: "Punjab", city: "Rawalpindi",
    phone: "+92-300-1234567", email1: "alnoor@gmail.com", email2: "", email3: "",
    maps_link: "https://maps.google.com", reviews_count: 45, website_url: "https://alnoor.pk",
    website_quality: "Good", facebook_url: "https://facebook.com/alnoor", fb_followers: 1200,
    instagram_url: "https://instagram.com/alnoor", ig_followers: 800,
    linkedin_url: "", li_followers: 0, social_media: "Facebook, Instagram",
    issues_found: "No website contact form", service_to_offer: "Web Development",
    contacted: "Yes", response: "Interested", follow_up_date: "2025-05-20", status: "Hot Lead",
    notes: "Owner is Ali, call after 10am"
  },
  {
    id: 2, sno: 2, date: "2025-05-03", business_name: "Karachi Sweets", category: "Food & Beverage",
    address: "Plot 5, Commercial Area", area: "DHA", state: "Sindh", city: "Karachi",
    phone: "+92-21-3456789", email1: "info@karachisweets.com", email2: "sales@karachisweets.com", email3: "",
    maps_link: "https://maps.google.com", reviews_count: 230, website_url: "",
    website_quality: "None", facebook_url: "https://facebook.com/karachisweets", fb_followers: 5600,
    instagram_url: "https://instagram.com/karachisweets", ig_followers: 3200,
    linkedin_url: "", li_followers: 0, social_media: "Facebook, Instagram",
    issues_found: "No website", service_to_offer: "Website + Social Media",
    contacted: "No", response: "", follow_up_date: "2025-05-25", status: "New",
    notes: ""
  },
  {
    id: 3, sno: 3, date: "2025-05-05", business_name: "Lahore Tech Hub", category: "IT Services",
    address: "2nd Floor, Gulberg III", area: "Gulberg", state: "Punjab", city: "Lahore",
    phone: "+92-42-1234567", email1: "contact@lahoretechhub.com", email2: "", email3: "",
    maps_link: "https://maps.google.com", reviews_count: 89, website_url: "https://lahoretechhub.com",
    website_quality: "Average", facebook_url: "", fb_followers: 0,
    instagram_url: "", ig_followers: 0,
    linkedin_url: "https://linkedin.com/company/lahoretechhub", li_followers: 450,
    social_media: "LinkedIn", issues_found: "Outdated website, no SEO",
    service_to_offer: "SEO + Website Redesign",
    contacted: "Yes", response: "Call back later", follow_up_date: "2025-05-18", status: "Follow-up",
    notes: "Send proposal first"
  },
];

const MOCK_USERS = [
  { id: 1, name: "Usman (You)", email: "usman@company.com", role: "Admin", status: "Approved", avatar: "U" },
  { id: 2, name: "Ahmed Khan", email: "ahmed@company.com", role: "Sales", status: "Approved", avatar: "A" },
  { id: 3, name: "Sara Malik", email: "sara@company.com", role: "Viewer", status: "Pending", avatar: "S" },
  { id: 4, name: "Bilal Raza", email: "bilal@company.com", role: "Sales", status: "Pending", avatar: "B" },
];

const STATUS_COLORS = {
  "Hot Lead": { bg: "#FEF3C7", text: "#92400E", border: "#F59E0B" },
  "New": { bg: "#EFF6FF", text: "#1E40AF", border: "#3B82F6" },
  "Follow-up": { bg: "#F0FDF4", text: "#166534", border: "#22C55E" },
  "Closed": { bg: "#F1F5F9", text: "#475569", border: "#94A3B8" },
  "Not Interested": { bg: "#FFF1F2", text: "#9F1239", border: "#F43F5E" },
};

const EMAIL_TEMPLATES = {
  "Web Development": `Subject: Boost Your Online Presence - Web Development Services\n\nDear [Business Name] Team,\n\nI came across your business on Google Maps and was impressed by your reviews. I noticed your online presence could be enhanced.\n\nWe specialize in:\n✓ Professional websites\n✓ Mobile-responsive design\n✓ SEO optimization\n\nWould you be open to a quick call this week?\n\nBest regards,\n[Your Name]`,
  "SEO + Website Redesign": `Subject: Your Website is Missing Customers - Let's Fix That\n\nDear [Business Name] Team,\n\nYour business has great reviews (${45} on Google), but your website isn't converting visitors.\n\nWe can help with:\n✓ Modern website redesign\n✓ Google SEO ranking\n✓ Lead generation\n\nLet's chat?\n\nBest regards,\n[Your Name]`,
  "Social Media": `Subject: Grow Your Social Media Following\n\nDear [Business Name] Team,\n\nYour social media has great potential! We help businesses like yours grow their following and engagement.\n\nOur services:\n✓ Content creation\n✓ Paid ads management\n✓ Analytics & reporting\n\nInterested in a free audit?\n\nBest regards,\n[Your Name]`,
};

// ─── MAIN APP ─────────────────────────────────────────────────────────────────
export default function LeadCRM() {
  const [currentUser] = useState(MOCK_USERS[0]);
  const [leads, setLeads] = useState(MOCK_LEADS);
  const [users, setUsers] = useState(MOCK_USERS);
  const [activeTab, setActiveTab] = useState("dashboard");
  const [selectedLead, setSelectedLead] = useState(null);
  const [showEmailModal, setShowEmailModal] = useState(false);
  const [showImportModal, setShowImportModal] = useState(false);
  const [showAddLeadModal, setShowAddLeadModal] = useState(false);
  const [filterStatus, setFilterStatus] = useState("All");
  const [filterCity, setFilterCity] = useState("All");
  const [searchTerm, setSearchTerm] = useState("");
  const [viewMode, setViewMode] = useState("table");
  const [emailDraft, setEmailDraft] = useState({ to: "", subject: "", body: "", template: "" });
  const [notification, setNotification] = useState(null);
  const [activityLog, setActivityLog] = useState([
    { id: 1, user: "Ahmed Khan", action: "contacted Al-Noor Traders via email", time: "2 hours ago" },
    { id: 2, user: "Usman", action: "added 3 new leads from Karachi", time: "5 hours ago" },
    { id: 3, user: "Ahmed Khan", action: "updated status: Lahore Tech Hub → Follow-up", time: "1 day ago" },
  ]);

  const showNotif = (msg, type = "success") => {
    setNotification({ msg, type });
    setTimeout(() => setNotification(null), 3000);
  };

  const addActivity = (action) => {
    setActivityLog(prev => [{ id: Date.now(), user: currentUser.name, action, time: "just now" }, ...prev]);
  };

  // Stats
  const stats = useMemo(() => ({
    total: leads.length,
    hot: leads.filter(l => l.status === "Hot Lead").length,
    contacted: leads.filter(l => l.contacted === "Yes").length,
    followUp: leads.filter(l => l.status === "Follow-up").length,
    responseRate: leads.length ? Math.round((leads.filter(l => l.response).length / leads.length) * 100) : 0,
  }), [leads]);

  // Filtered leads
  const filteredLeads = useMemo(() => {
    return leads.filter(l => {
      const matchStatus = filterStatus === "All" || l.status === filterStatus;
      const matchCity = filterCity === "All" || l.city === filterCity;
      const matchSearch = !searchTerm ||
        l.business_name.toLowerCase().includes(searchTerm.toLowerCase()) ||
        l.category.toLowerCase().includes(searchTerm.toLowerCase()) ||
        l.city.toLowerCase().includes(searchTerm.toLowerCase());
      return matchStatus && matchCity && matchSearch;
    });
  }, [leads, filterStatus, filterCity, searchTerm]);

  const cities = [...new Set(leads.map(l => l.city))];

  const updateLeadStatus = (id, field, value) => {
    setLeads(prev => prev.map(l => l.id === id ? { ...l, [field]: value } : l));
    addActivity(`updated ${field} for lead #${id}`);
    showNotif("Lead updated!");
  };

  const openEmailModal = (lead) => {
    const template = EMAIL_TEMPLATES[lead.service_to_offer] || EMAIL_TEMPLATES["Web Development"];
    setEmailDraft({
      to: lead.email1,
      subject: `Grow ${lead.business_name} Online`,
      body: template.replace("[Business Name]", lead.business_name),
      leadId: lead.id
    });
    setShowEmailModal(true);
  };

  const sendEmail = () => {
    const mailto = `mailto:${emailDraft.to}?subject=${encodeURIComponent(emailDraft.subject)}&body=${encodeURIComponent(emailDraft.body)}`;
    window.open(mailto);
    updateLeadStatus(emailDraft.leadId, "contacted", "Yes");
    addActivity(`sent email to ${emailDraft.to}`);
    setShowEmailModal(false);
    showNotif("Email client opened! Lead marked as contacted.");
  };

  const handleCSVImport = (text) => {
    const lines = text.trim().split("\n");
    const headers = lines[0].split(",").map(h => h.trim().toLowerCase().replace(/ /g, "_"));
    const newLeads = lines.slice(1).map((line, i) => {
      const vals = line.split(",");
      const obj = { id: Date.now() + i };
      headers.forEach((h, j) => obj[h] = vals[j]?.trim() || "");
      return { ...obj, status: obj.status || "New", contacted: obj.contacted || "No" };
    }).filter(l => l.business_name);
    setLeads(prev => [...prev, ...newLeads]);
    addActivity(`imported ${newLeads.length} leads from CSV`);
    setShowImportModal(false);
    showNotif(`${newLeads.length} new leads imported!`);
  };

  const approveUser = (id) => {
    setUsers(prev => prev.map(u => u.id === id ? { ...u, status: "Approved" } : u));
    showNotif("User approved!");
    addActivity(`approved user access`);
  };

  return (
    <div style={{ minHeight: "100vh", background: "var(--color-background-tertiary)", fontFamily: "'DM Sans', 'Segoe UI', sans-serif" }}>
      {/* Google Fonts */}
      <link href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;600;700&display=swap" rel="stylesheet" />

      {/* Notification */}
      {notification && (
        <div style={{
          position: "fixed", top: 20, right: 20, zIndex: 9999,
          background: notification.type === "success" ? "#10B981" : "#EF4444",
          color: "#fff", padding: "12px 20px", borderRadius: 10,
          fontWeight: 600, fontSize: 14, boxShadow: "0 4px 20px rgba(0,0,0,0.15)",
          display: "flex", alignItems: "center", gap: 8
        }}>
          {notification.type === "success" ? "✓" : "✗"} {notification.msg}
        </div>
      )}

      {/* SIDEBAR */}
      <div style={{
        position: "fixed", left: 0, top: 0, bottom: 0, width: 240,
        background: "#0F172A", color: "#fff", display: "flex", flexDirection: "column",
        zIndex: 100, padding: "0"
      }}>
        {/* Logo */}
        <div style={{ padding: "24px 20px 20px", borderBottom: "1px solid rgba(255,255,255,0.08)" }}>
          <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
            <div style={{ width: 36, height: 36, background: "linear-gradient(135deg, #3B82F6, #8B5CF6)", borderRadius: 10, display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 700, fontSize: 16 }}>L</div>
            <div>
              <div style={{ fontWeight: 700, fontSize: 15, letterSpacing: "-0.3px" }}>LeadPro CRM</div>
              <div style={{ fontSize: 11, color: "rgba(255,255,255,0.4)", marginTop: 1 }}>Marketing Platform</div>
            </div>
          </div>
        </div>

        {/* Nav */}
        <nav style={{ flex: 1, padding: "16px 12px", overflowY: "auto" }}>
          {[
            { id: "dashboard", icon: "⬛", label: "Dashboard" },
            { id: "leads", icon: "👥", label: "All Leads" },
            { id: "pipeline", icon: "📊", label: "Pipeline" },
            { id: "outreach", icon: "📧", label: "Outreach" },
            { id: "team", icon: "🔑", label: "Team & Access" },
            { id: "activity", icon: "🕐", label: "Activity Log" },
            { id: "settings", icon: "⚙️", label: "Settings" },
          ].map(item => (
            <button key={item.id} onClick={() => setActiveTab(item.id)} style={{
              width: "100%", textAlign: "left", padding: "10px 14px", marginBottom: 4,
              background: activeTab === item.id ? "rgba(59,130,246,0.15)" : "transparent",
              border: activeTab === item.id ? "1px solid rgba(59,130,246,0.3)" : "1px solid transparent",
              borderRadius: 8, color: activeTab === item.id ? "#60A5FA" : "rgba(255,255,255,0.6)",
              cursor: "pointer", fontSize: 13, fontWeight: activeTab === item.id ? 600 : 400,
              display: "flex", alignItems: "center", gap: 10, transition: "all 0.15s"
            }}>
              <span style={{ fontSize: 15 }}>{item.icon}</span> {item.label}
              {item.id === "team" && users.filter(u => u.status === "Pending").length > 0 && (
                <span style={{ marginLeft: "auto", background: "#EF4444", color: "#fff", borderRadius: 10, padding: "1px 7px", fontSize: 11, fontWeight: 700 }}>
                  {users.filter(u => u.status === "Pending").length}
                </span>
              )}
            </button>
          ))}
        </nav>

        {/* User */}
        <div style={{ padding: "16px 20px", borderTop: "1px solid rgba(255,255,255,0.08)" }}>
          <div style={{ display: "flex", alignItems: "center", gap: 10 }}>
            <div style={{ width: 32, height: 32, borderRadius: "50%", background: "linear-gradient(135deg, #3B82F6, #8B5CF6)", display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 700, fontSize: 13 }}>U</div>
            <div>
              <div style={{ fontSize: 13, fontWeight: 600 }}>Usman (Admin)</div>
              <div style={{ fontSize: 11, color: "rgba(255,255,255,0.4)" }}>usman@company.com</div>
            </div>
          </div>
        </div>
      </div>

      {/* MAIN CONTENT */}
      <div style={{ marginLeft: 240, padding: "28px 32px", minHeight: "100vh" }}>

        {/* ── DASHBOARD ── */}
        {activeTab === "dashboard" && (
          <div>
            <div style={{ marginBottom: 28 }}>
              <h1 style={{ fontSize: 26, fontWeight: 700, color: "var(--color-text-primary)", margin: 0 }}>Dashboard</h1>
              <p style={{ color: "var(--color-text-secondary)", marginTop: 4, fontSize: 14 }}>Welcome back! Here's your lead overview.</p>
            </div>

            {/* Stats Grid */}
            <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(170px, 1fr))", gap: 16, marginBottom: 28 }}>
              {[
                { label: "Total Leads", value: stats.total, icon: "👥", color: "#3B82F6", bg: "#EFF6FF" },
                { label: "Hot Leads 🔥", value: stats.hot, icon: "🔥", color: "#F59E0B", bg: "#FEF3C7" },
                { label: "Contacted", value: stats.contacted, icon: "✉️", color: "#10B981", bg: "#ECFDF5" },
                { label: "Follow-ups Today", value: stats.followUp, icon: "📅", color: "#8B5CF6", bg: "#F5F3FF" },
                { label: "Response Rate", value: stats.responseRate + "%", icon: "📈", color: "#EC4899", bg: "#FDF2F8" },
              ].map(s => (
                <div key={s.label} style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, padding: "18px 20px", borderLeft: `3px solid ${s.color}` }}>
                  <div style={{ fontSize: 11, color: "var(--color-text-secondary)", fontWeight: 600, textTransform: "uppercase", letterSpacing: "0.5px", marginBottom: 8 }}>{s.label}</div>
                  <div style={{ fontSize: 32, fontWeight: 700, color: s.color }}>{s.value}</div>
                </div>
              ))}
            </div>

            {/* Recent Leads + Follow-ups */}
            <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 20 }}>
              {/* Recent Leads */}
              <div style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, padding: 20 }}>
                <div style={{ fontWeight: 700, fontSize: 15, marginBottom: 16, color: "var(--color-text-primary)" }}>Recent Leads</div>
                {leads.slice(0, 4).map(lead => (
                  <div key={lead.id} onClick={() => { setSelectedLead(lead); setActiveTab("leads"); }}
                    style={{ display: "flex", alignItems: "center", gap: 12, padding: "10px 0", borderBottom: "0.5px solid var(--color-border-tertiary)", cursor: "pointer" }}>
                    <div style={{ width: 38, height: 38, borderRadius: 10, background: "#EFF6FF", display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 700, fontSize: 14, color: "#3B82F6", flexShrink: 0 }}>
                      {lead.business_name[0]}
                    </div>
                    <div style={{ flex: 1, minWidth: 0 }}>
                      <div style={{ fontWeight: 600, fontSize: 13, color: "var(--color-text-primary)", whiteSpace: "nowrap", overflow: "hidden", textOverflow: "ellipsis" }}>{lead.business_name}</div>
                      <div style={{ fontSize: 12, color: "var(--color-text-secondary)" }}>{lead.category} · {lead.city}</div>
                    </div>
                    <StatusBadge status={lead.status} />
                  </div>
                ))}
              </div>

              {/* Today's Follow-ups */}
              <div style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, padding: 20 }}>
                <div style={{ fontWeight: 700, fontSize: 15, marginBottom: 16, color: "var(--color-text-primary)" }}>Today's Follow-ups</div>
                {leads.filter(l => l.status === "Follow-up").map(lead => (
                  <div key={lead.id} style={{ display: "flex", alignItems: "center", gap: 12, padding: "10px 0", borderBottom: "0.5px solid var(--color-border-tertiary)" }}>
                    <div style={{ width: 38, height: 38, borderRadius: 10, background: "#F0FDF4", display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 700, fontSize: 14, color: "#10B981", flexShrink: 0 }}>
                      {lead.business_name[0]}
                    </div>
                    <div style={{ flex: 1 }}>
                      <div style={{ fontWeight: 600, fontSize: 13, color: "var(--color-text-primary)" }}>{lead.business_name}</div>
                      <div style={{ fontSize: 12, color: "var(--color-text-secondary)" }}>📅 {lead.follow_up_date}</div>
                    </div>
                    <button onClick={() => openEmailModal(lead)} style={{ background: "#EFF6FF", border: "none", borderRadius: 7, padding: "6px 12px", color: "#3B82F6", fontWeight: 600, fontSize: 12, cursor: "pointer" }}>
                      Email
                    </button>
                  </div>
                ))}
                {leads.filter(l => l.status === "Follow-up").length === 0 && (
                  <div style={{ color: "var(--color-text-secondary)", fontSize: 13, textAlign: "center", padding: "20px 0" }}>No follow-ups scheduled 🎉</div>
                )}
              </div>
            </div>

            {/* City Breakdown */}
            <div style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, padding: 20, marginTop: 20 }}>
              <div style={{ fontWeight: 700, fontSize: 15, marginBottom: 16, color: "var(--color-text-primary)" }}>Leads by City</div>
              <div style={{ display: "flex", gap: 12, flexWrap: "wrap" }}>
                {cities.map(city => {
                  const count = leads.filter(l => l.city === city).length;
                  const pct = Math.round((count / leads.length) * 100);
                  return (
                    <div key={city} style={{ flex: "1 1 150px", background: "var(--color-background-secondary)", borderRadius: 10, padding: "14px 16px" }}>
                      <div style={{ fontWeight: 600, fontSize: 13, color: "var(--color-text-primary)", marginBottom: 8 }}>{city}</div>
                      <div style={{ background: "var(--color-border-tertiary)", borderRadius: 4, height: 6, marginBottom: 6 }}>
                        <div style={{ background: "#3B82F6", height: 6, borderRadius: 4, width: pct + "%" }} />
                      </div>
                      <div style={{ fontSize: 12, color: "var(--color-text-secondary)" }}>{count} leads · {pct}%</div>
                    </div>
                  );
                })}
              </div>
            </div>
          </div>
        )}

        {/* ── LEADS TABLE ── */}
        {activeTab === "leads" && (
          <div>
            <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", marginBottom: 24 }}>
              <div>
                <h1 style={{ fontSize: 24, fontWeight: 700, color: "var(--color-text-primary)", margin: 0 }}>All Leads</h1>
                <p style={{ color: "var(--color-text-secondary)", marginTop: 4, fontSize: 14 }}>{filteredLeads.length} leads found</p>
              </div>
              <div style={{ display: "flex", gap: 10 }}>
                <button onClick={() => setShowImportModal(true)} style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-secondary)", borderRadius: 9, padding: "9px 16px", fontSize: 13, fontWeight: 600, cursor: "pointer", color: "var(--color-text-primary)", display: "flex", alignItems: "center", gap: 6 }}>
                  📥 Import CSV
                </button>
                <button onClick={() => setShowAddLeadModal(true)} style={{ background: "#3B82F6", border: "none", borderRadius: 9, padding: "9px 18px", fontSize: 13, fontWeight: 600, cursor: "pointer", color: "#fff", display: "flex", alignItems: "center", gap: 6 }}>
                  + Add Lead
                </button>
              </div>
            </div>

            {/* Filters */}
            <div style={{ display: "flex", gap: 12, marginBottom: 16, flexWrap: "wrap" }}>
              <input placeholder="🔍 Search business, city, category..." value={searchTerm}
                onChange={e => setSearchTerm(e.target.value)}
                style={{ flex: "1 1 280px", padding: "9px 14px", borderRadius: 9, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-primary)", fontSize: 13, color: "var(--color-text-primary)" }} />
              <select value={filterStatus} onChange={e => setFilterStatus(e.target.value)}
                style={{ padding: "9px 14px", borderRadius: 9, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-primary)", fontSize: 13, color: "var(--color-text-primary)" }}>
                <option>All</option>
                {Object.keys(STATUS_COLORS).map(s => <option key={s}>{s}</option>)}
              </select>
              <select value={filterCity} onChange={e => setFilterCity(e.target.value)}
                style={{ padding: "9px 14px", borderRadius: 9, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-primary)", fontSize: 13, color: "var(--color-text-primary)" }}>
                <option>All</option>
                {cities.map(c => <option key={c}>{c}</option>)}
              </select>
              <div style={{ display: "flex", border: "0.5px solid var(--color-border-secondary)", borderRadius: 9, overflow: "hidden" }}>
                {["table", "kanban"].map(m => (
                  <button key={m} onClick={() => setViewMode(m)} style={{ padding: "9px 14px", border: "none", background: viewMode === m ? "#3B82F6" : "var(--color-background-primary)", color: viewMode === m ? "#fff" : "var(--color-text-secondary)", fontSize: 12, fontWeight: 600, cursor: "pointer", textTransform: "capitalize" }}>
                    {m === "table" ? "☰ Table" : "⬛ Kanban"}
                  </button>
                ))}
              </div>
            </div>

            {viewMode === "table" ? (
              <LeadsTable leads={filteredLeads} onEmail={openEmailModal} onUpdate={updateLeadStatus} onSelect={setSelectedLead} />
            ) : (
              <KanbanView leads={filteredLeads} onEmail={openEmailModal} onUpdate={updateLeadStatus} />
            )}
          </div>
        )}

        {/* ── PIPELINE ── */}
        {activeTab === "pipeline" && (
          <div>
            <div style={{ marginBottom: 24 }}>
              <h1 style={{ fontSize: 24, fontWeight: 700, color: "var(--color-text-primary)", margin: 0 }}>Pipeline View</h1>
              <p style={{ color: "var(--color-text-secondary)", marginTop: 4, fontSize: 14 }}>Drag leads between stages</p>
            </div>
            <KanbanView leads={leads} onEmail={openEmailModal} onUpdate={updateLeadStatus} />
          </div>
        )}

        {/* ── OUTREACH ── */}
        {activeTab === "outreach" && (
          <div>
            <div style={{ marginBottom: 24 }}>
              <h1 style={{ fontSize: 24, fontWeight: 700, color: "var(--color-text-primary)", margin: 0 }}>Outreach Center</h1>
              <p style={{ color: "var(--color-text-secondary)", marginTop: 4, fontSize: 14 }}>Email & social media outreach for all leads</p>
            </div>
            <div style={{ display: "grid", gap: 14 }}>
              {leads.filter(l => l.contacted !== "Yes").map(lead => (
                <OutreachCard key={lead.id} lead={lead} onEmail={() => openEmailModal(lead)} onUpdate={updateLeadStatus} />
              ))}
              {leads.filter(l => l.contacted !== "Yes").length === 0 && (
                <div style={{ textAlign: "center", padding: "40px", background: "var(--color-background-primary)", borderRadius: 14, color: "var(--color-text-secondary)" }}>
                  🎉 All leads have been contacted!
                </div>
              )}
            </div>
          </div>
        )}

        {/* ── TEAM ── */}
        {activeTab === "team" && (
          <div>
            <div style={{ marginBottom: 24 }}>
              <h1 style={{ fontSize: 24, fontWeight: 700, color: "var(--color-text-primary)", margin: 0 }}>Team & Access Control</h1>
              <p style={{ color: "var(--color-text-secondary)", marginTop: 4, fontSize: 14 }}>Approve team members and manage roles</p>
            </div>

            {users.filter(u => u.status === "Pending").length > 0 && (
              <div style={{ background: "#FEF3C7", border: "1px solid #F59E0B", borderRadius: 12, padding: "14px 18px", marginBottom: 20, display: "flex", alignItems: "center", gap: 10 }}>
                <span style={{ fontSize: 18 }}>⚠️</span>
                <span style={{ fontWeight: 600, color: "#92400E", fontSize: 13 }}>
                  {users.filter(u => u.status === "Pending").length} user(s) waiting for your approval
                </span>
              </div>
            )}

            <div style={{ display: "grid", gap: 12 }}>
              {users.map(user => (
                <div key={user.id} style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, padding: "18px 20px", display: "flex", alignItems: "center", gap: 16 }}>
                  <div style={{ width: 46, height: 46, borderRadius: "50%", background: user.status === "Approved" ? "#EFF6FF" : "#FEF3C7", display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 700, fontSize: 18, color: user.status === "Approved" ? "#3B82F6" : "#F59E0B", flexShrink: 0 }}>
                    {user.avatar}
                  </div>
                  <div style={{ flex: 1 }}>
                    <div style={{ fontWeight: 700, fontSize: 15, color: "var(--color-text-primary)" }}>{user.name}</div>
                    <div style={{ fontSize: 13, color: "var(--color-text-secondary)", marginTop: 2 }}>{user.email}</div>
                  </div>
                  <div style={{ display: "flex", alignItems: "center", gap: 12 }}>
                    <span style={{ background: user.role === "Admin" ? "#EDE9FE" : user.role === "Sales" ? "#ECFDF5" : "#F1F5F9", color: user.role === "Admin" ? "#6D28D9" : user.role === "Sales" ? "#065F46" : "#475569", borderRadius: 6, padding: "4px 10px", fontSize: 12, fontWeight: 600 }}>
                      {user.role}
                    </span>
                    {user.status === "Pending" ? (
                      <div style={{ display: "flex", gap: 8 }}>
                        <button onClick={() => approveUser(user.id)} style={{ background: "#10B981", border: "none", borderRadius: 7, padding: "7px 14px", color: "#fff", fontWeight: 600, fontSize: 12, cursor: "pointer" }}>
                          ✓ Approve
                        </button>
                        <button onClick={() => setUsers(prev => prev.filter(u => u.id !== user.id))} style={{ background: "#FEF2F2", border: "1px solid #FCA5A5", borderRadius: 7, padding: "7px 14px", color: "#EF4444", fontWeight: 600, fontSize: 12, cursor: "pointer" }}>
                          ✗ Reject
                        </button>
                      </div>
                    ) : (
                      <span style={{ background: "#ECFDF5", color: "#065F46", borderRadius: 6, padding: "4px 10px", fontSize: 12, fontWeight: 600 }}>✓ Active</span>
                    )}
                  </div>
                </div>
              ))}
            </div>

            <div style={{ marginTop: 20, background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, padding: 20 }}>
              <div style={{ fontWeight: 700, fontSize: 15, marginBottom: 12, color: "var(--color-text-primary)" }}>🔗 Google Sheet Sync</div>
              <p style={{ color: "var(--color-text-secondary)", fontSize: 13, lineHeight: 1.6 }}>
                Aapke employees Google Sheet mein leads add karein — yeh automatically CRM mein aa jayengi.<br />
                Neeche diya script aapki Google Sheet mein paste karein (Extensions → Apps Script):
              </p>
              <pre style={{ background: "var(--color-background-secondary)", borderRadius: 10, padding: 16, fontSize: 12, overflowX: "auto", color: "var(--color-text-secondary)", marginTop: 10 }}>
{`function onEdit(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet()
    .getActiveSheet();
  var lastRow = sheet.getLastRow();
  var data = sheet.getRange(lastRow, 1, 1, 
    sheet.getLastColumn()).getValues()[0];
  
  var url = "YOUR_SUPABASE_URL/rest/v1/leads";
  var options = {
    method: "POST",
    headers: {
      "apikey": "YOUR_SUPABASE_ANON_KEY",
      "Content-Type": "application/json"
    },
    payload: JSON.stringify({
      business_name: data[2],
      category: data[3],
      city: data[7],
      phone: data[8],
      email1: data[9],
      status: "New"
    })
  };
  UrlFetchApp.fetch(url, options);
}`}
              </pre>
            </div>
          </div>
        )}

        {/* ── ACTIVITY LOG ── */}
        {activeTab === "activity" && (
          <div>
            <div style={{ marginBottom: 24 }}>
              <h1 style={{ fontSize: 24, fontWeight: 700, color: "var(--color-text-primary)", margin: 0 }}>Activity Log</h1>
              <p style={{ color: "var(--color-text-secondary)", marginTop: 4, fontSize: 14 }}>Track everything your team does</p>
            </div>
            <div style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, overflow: "hidden" }}>
              {activityLog.map((log, i) => (
                <div key={log.id} style={{ display: "flex", alignItems: "center", gap: 14, padding: "14px 20px", borderBottom: i < activityLog.length - 1 ? "0.5px solid var(--color-border-tertiary)" : "none" }}>
                  <div style={{ width: 36, height: 36, borderRadius: "50%", background: "#EFF6FF", display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 700, fontSize: 13, color: "#3B82F6", flexShrink: 0 }}>
                    {log.user[0]}
                  </div>
                  <div style={{ flex: 1 }}>
                    <span style={{ fontWeight: 600, fontSize: 13, color: "var(--color-text-primary)" }}>{log.user}</span>
                    <span style={{ fontSize: 13, color: "var(--color-text-secondary)" }}> {log.action}</span>
                  </div>
                  <div style={{ fontSize: 12, color: "var(--color-text-tertiary)", flexShrink: 0 }}>{log.time}</div>
                </div>
              ))}
            </div>
          </div>
        )}

        {/* ── SETTINGS ── */}
        {activeTab === "settings" && (
          <div>
            <div style={{ marginBottom: 24 }}>
              <h1 style={{ fontSize: 24, fontWeight: 700, color: "var(--color-text-primary)", margin: 0 }}>Settings</h1>
            </div>
            <div style={{ display: "grid", gap: 16 }}>
              {[
                { title: "📧 Email Integration", desc: "Connect your Gmail or Outlook for one-click sending", btn: "Connect Email" },
                { title: "📱 WhatsApp Integration", desc: "Send WhatsApp messages directly from CRM", btn: "Setup WhatsApp" },
                { title: "🗄️ Supabase Database", desc: "Connect your Supabase for persistent storage", btn: "Connect DB" },
                { title: "📊 Google Sheets Sync", desc: "Auto-import leads from your Google Sheet", btn: "Configure" },
                { title: "🔔 Follow-up Reminders", desc: "Get email/SMS reminders for scheduled follow-ups", btn: "Setup Reminders" },
              ].map(s => (
                <div key={s.title} style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, padding: "18px 20px", display: "flex", alignItems: "center", gap: 16 }}>
                  <div style={{ flex: 1 }}>
                    <div style={{ fontWeight: 700, fontSize: 14, color: "var(--color-text-primary)" }}>{s.title}</div>
                    <div style={{ fontSize: 13, color: "var(--color-text-secondary)", marginTop: 4 }}>{s.desc}</div>
                  </div>
                  <button style={{ background: "#3B82F6", border: "none", borderRadius: 8, padding: "8px 16px", color: "#fff", fontWeight: 600, fontSize: 12, cursor: "pointer", whiteSpace: "nowrap" }}>
                    {s.btn}
                  </button>
                </div>
              ))}
            </div>
          </div>
        )}
      </div>

      {/* ── EMAIL MODAL ── */}
      {showEmailModal && (
        <Modal title="📧 Send Email" onClose={() => setShowEmailModal(false)}>
          <div style={{ display: "grid", gap: 12 }}>
            <div>
              <label style={{ fontSize: 12, fontWeight: 600, color: "var(--color-text-secondary)", display: "block", marginBottom: 6 }}>TO</label>
              <input value={emailDraft.to} onChange={e => setEmailDraft(p => ({ ...p, to: e.target.value }))}
                style={{ width: "100%", padding: "9px 12px", borderRadius: 8, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-secondary)", fontSize: 13, color: "var(--color-text-primary)", boxSizing: "border-box" }} />
            </div>
            <div>
              <label style={{ fontSize: 12, fontWeight: 600, color: "var(--color-text-secondary)", display: "block", marginBottom: 6 }}>SUBJECT</label>
              <input value={emailDraft.subject} onChange={e => setEmailDraft(p => ({ ...p, subject: e.target.value }))}
                style={{ width: "100%", padding: "9px 12px", borderRadius: 8, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-secondary)", fontSize: 13, color: "var(--color-text-primary)", boxSizing: "border-box" }} />
            </div>
            <div>
              <label style={{ fontSize: 12, fontWeight: 600, color: "var(--color-text-secondary)", display: "block", marginBottom: 6 }}>MESSAGE</label>
              <textarea value={emailDraft.body} onChange={e => setEmailDraft(p => ({ ...p, body: e.target.value }))} rows={8}
                style={{ width: "100%", padding: "9px 12px", borderRadius: 8, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-secondary)", fontSize: 13, color: "var(--color-text-primary)", resize: "vertical", boxSizing: "border-box", fontFamily: "inherit", lineHeight: 1.6 }} />
            </div>
            <div style={{ display: "flex", gap: 10, justifyContent: "flex-end", marginTop: 4 }}>
              <button onClick={() => setShowEmailModal(false)} style={{ padding: "9px 18px", borderRadius: 8, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-primary)", cursor: "pointer", fontSize: 13, fontWeight: 600, color: "var(--color-text-primary)" }}>Cancel</button>
              <button onClick={sendEmail} style={{ padding: "9px 20px", borderRadius: 8, border: "none", background: "#3B82F6", color: "#fff", cursor: "pointer", fontSize: 13, fontWeight: 700 }}>📧 Open in Email</button>
            </div>
          </div>
        </Modal>
      )}

      {/* ── IMPORT MODAL ── */}
      {showImportModal && (
        <Modal title="📥 Import Leads from CSV" onClose={() => setShowImportModal(false)}>
          <p style={{ color: "var(--color-text-secondary)", fontSize: 13, lineHeight: 1.6, marginBottom: 16 }}>
            Apni Google Sheet se CSV export karein aur yahan paste karein. First row headers honi chahiye.
          </p>
          <textarea id="csv-input" rows={10} placeholder={"S.No,Date,Business Name,Category,...\n1,2025-05-01,Al-Noor Traders,..."}
            style={{ width: "100%", padding: "10px 12px", borderRadius: 8, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-secondary)", fontSize: 12, fontFamily: "monospace", color: "var(--color-text-primary)", resize: "vertical", boxSizing: "border-box" }} />
          <div style={{ display: "flex", gap: 10, justifyContent: "flex-end", marginTop: 12 }}>
            <button onClick={() => setShowImportModal(false)} style={{ padding: "9px 18px", borderRadius: 8, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-primary)", cursor: "pointer", fontSize: 13, fontWeight: 600, color: "var(--color-text-primary)" }}>Cancel</button>
            <button onClick={() => handleCSVImport(document.getElementById("csv-input").value)} style={{ padding: "9px 20px", borderRadius: 8, border: "none", background: "#10B981", color: "#fff", cursor: "pointer", fontSize: 13, fontWeight: 700 }}>✓ Import Leads</button>
          </div>
        </Modal>
      )}

      {/* ── ADD LEAD MODAL ── */}
      {showAddLeadModal && (
        <AddLeadModal onClose={() => setShowAddLeadModal(false)} onAdd={(lead) => {
          const newLead = { ...lead, id: Date.now(), status: "New", contacted: "No" };
          setLeads(prev => [...prev, newLead]);
          addActivity(`added lead: ${lead.business_name}`);
          setShowAddLeadModal(false);
          showNotif("New lead added!");
        }} />
      )}
    </div>
  );
}

// ─── SUB-COMPONENTS ───────────────────────────────────────────────────────────

function StatusBadge({ status }) {
  const c = STATUS_COLORS[status] || STATUS_COLORS["New"];
  return (
    <span style={{ background: c.bg, color: c.text, border: `1px solid ${c.border}`, borderRadius: 6, padding: "3px 9px", fontSize: 11, fontWeight: 700, whiteSpace: "nowrap" }}>
      {status}
    </span>
  );
}

function LeadsTable({ leads, onEmail, onUpdate, onSelect }) {
  const [expandedId, setExpandedId] = useState(null);
  return (
    <div style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, overflow: "hidden" }}>
      <div style={{ overflowX: "auto" }}>
        <table style={{ width: "100%", borderCollapse: "collapse", fontSize: 13 }}>
          <thead>
            <tr style={{ background: "var(--color-background-secondary)", borderBottom: "0.5px solid var(--color-border-tertiary)" }}>
              {["#", "Business", "Category", "City", "Phone", "Status", "Contacted", "Actions"].map(h => (
                <th key={h} style={{ padding: "11px 14px", textAlign: "left", fontWeight: 600, color: "var(--color-text-secondary)", fontSize: 12, whiteSpace: "nowrap" }}>{h}</th>
              ))}
            </tr>
          </thead>
          <tbody>
            {leads.map((lead, i) => (
              <>
                <tr key={lead.id} style={{ borderBottom: "0.5px solid var(--color-border-tertiary)", transition: "background 0.1s" }}
                  onMouseEnter={e => e.currentTarget.style.background = "var(--color-background-secondary)"}
                  onMouseLeave={e => e.currentTarget.style.background = "transparent"}>
                  <td style={{ padding: "12px 14px", color: "var(--color-text-tertiary)", fontWeight: 500 }}>{i + 1}</td>
                  <td style={{ padding: "12px 14px" }}>
                    <button onClick={() => setExpandedId(expandedId === lead.id ? null : lead.id)}
                      style={{ background: "none", border: "none", cursor: "pointer", textAlign: "left", padding: 0 }}>
                      <div style={{ fontWeight: 600, color: "var(--color-text-primary)", fontSize: 13 }}>{lead.business_name}</div>
                      <div style={{ fontSize: 11, color: "var(--color-text-tertiary)", marginTop: 2 }}>{lead.address}</div>
                    </button>
                  </td>
                  <td style={{ padding: "12px 14px", color: "var(--color-text-secondary)" }}>{lead.category}</td>
                  <td style={{ padding: "12px 14px", color: "var(--color-text-secondary)" }}>{lead.city}</td>
                  <td style={{ padding: "12px 14px", color: "var(--color-text-secondary)", whiteSpace: "nowrap" }}>{lead.phone}</td>
                  <td style={{ padding: "12px 14px" }}>
                    <select value={lead.status} onChange={e => onUpdate(lead.id, "status", e.target.value)}
                      style={{ border: "none", background: "transparent", fontSize: 12, cursor: "pointer", fontWeight: 600, color: STATUS_COLORS[lead.status]?.text || "#3B82F6" }}>
                      {Object.keys(STATUS_COLORS).map(s => <option key={s}>{s}</option>)}
                    </select>
                  </td>
                  <td style={{ padding: "12px 14px" }}>
                    <span style={{ background: lead.contacted === "Yes" ? "#ECFDF5" : "#FEF3C7", color: lead.contacted === "Yes" ? "#065F46" : "#92400E", borderRadius: 5, padding: "3px 8px", fontSize: 11, fontWeight: 600 }}>
                      {lead.contacted === "Yes" ? "✓ Yes" : "○ No"}
                    </span>
                  </td>
                  <td style={{ padding: "12px 14px" }}>
                    <div style={{ display: "flex", gap: 6 }}>
                      {lead.email1 && (
                        <button onClick={() => onEmail(lead)} title="Email" style={{ background: "#EFF6FF", border: "none", borderRadius: 6, padding: "5px 9px", cursor: "pointer", fontSize: 13 }}>📧</button>
                      )}
                      {lead.facebook_url && (
                        <button onClick={() => window.open(lead.facebook_url)} title="Facebook" style={{ background: "#F0F2FF", border: "none", borderRadius: 6, padding: "5px 9px", cursor: "pointer", fontSize: 13 }}>f</button>
                      )}
                      {lead.instagram_url && (
                        <button onClick={() => window.open(lead.instagram_url)} title="Instagram" style={{ background: "#FFF0F6", border: "none", borderRadius: 6, padding: "5px 9px", cursor: "pointer", fontSize: 13 }}>📸</button>
                      )}
                      {lead.linkedin_url && (
                        <button onClick={() => window.open(lead.linkedin_url)} title="LinkedIn" style={{ background: "#EFF6FF", border: "none", borderRadius: 6, padding: "5px 9px", cursor: "pointer", fontSize: 13 }}>in</button>
                      )}
                      {lead.maps_link && (
                        <button onClick={() => window.open(lead.maps_link)} title="Maps" style={{ background: "#F0FDF4", border: "none", borderRadius: 6, padding: "5px 9px", cursor: "pointer", fontSize: 13 }}>📍</button>
                      )}
                    </div>
                  </td>
                </tr>
                {expandedId === lead.id && (
                  <tr key={lead.id + "-exp"} style={{ background: "var(--color-background-secondary)" }}>
                    <td colSpan={8} style={{ padding: "16px 20px" }}>
                      <div style={{ display: "grid", gridTemplateColumns: "repeat(auto-fit, minmax(200px, 1fr))", gap: 12, fontSize: 13 }}>
                        {[
                          ["Email 1", lead.email1], ["Email 2", lead.email2], ["Email 3", lead.email3],
                          ["Website", lead.website_url], ["Website Quality", lead.website_quality],
                          ["Reviews", lead.reviews_count], ["Service to Offer", lead.service_to_offer],
                          ["Issues Found", lead.issues_found], ["Response", lead.response],
                          ["Follow-up Date", lead.follow_up_date], ["Notes", lead.notes],
                          ["FB Followers", lead.fb_followers ? lead.fb_followers.toLocaleString() : ""],
                          ["IG Followers", lead.ig_followers ? lead.ig_followers.toLocaleString() : ""],
                        ].filter(([, v]) => v).map(([k, v]) => (
                          <div key={k}>
                            <div style={{ fontSize: 11, color: "var(--color-text-tertiary)", fontWeight: 600, textTransform: "uppercase", letterSpacing: "0.4px", marginBottom: 3 }}>{k}</div>
                            <div style={{ color: "var(--color-text-primary)", fontWeight: 500 }}>{v}</div>
                          </div>
                        ))}
                      </div>
                      {lead.notes && (
                        <div style={{ marginTop: 12, background: "#FEF9C3", borderRadius: 8, padding: "10px 14px", fontSize: 13, color: "#78350F" }}>
                          📝 {lead.notes}
                        </div>
                      )}
                    </td>
                  </tr>
                )}
              </>
            ))}
          </tbody>
        </table>
      </div>
      {leads.length === 0 && (
        <div style={{ padding: "40px", textAlign: "center", color: "var(--color-text-secondary)", fontSize: 14 }}>
          No leads found. Try adjusting your filters or import new leads.
        </div>
      )}
    </div>
  );
}

function KanbanView({ leads, onEmail, onUpdate }) {
  const columns = ["New", "Follow-up", "Hot Lead", "Closed", "Not Interested"];
  return (
    <div style={{ display: "flex", gap: 14, overflowX: "auto", paddingBottom: 8 }}>
      {columns.map(col => {
        const colLeads = leads.filter(l => l.status === col);
        const c = STATUS_COLORS[col];
        return (
          <div key={col} style={{ flex: "0 0 260px", background: "var(--color-background-secondary)", borderRadius: 12, padding: 14 }}>
            <div style={{ display: "flex", alignItems: "center", justifyContent: "space-between", marginBottom: 12 }}>
              <span style={{ fontWeight: 700, fontSize: 13, color: "var(--color-text-primary)" }}>{col}</span>
              <span style={{ background: c?.bg, color: c?.text, borderRadius: 5, padding: "2px 8px", fontSize: 11, fontWeight: 700 }}>{colLeads.length}</span>
            </div>
            <div style={{ display: "flex", flexDirection: "column", gap: 10, minHeight: 60 }}>
              {colLeads.map(lead => (
                <div key={lead.id} style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 10, padding: 12, cursor: "pointer" }}>
                  <div style={{ fontWeight: 600, fontSize: 13, color: "var(--color-text-primary)", marginBottom: 4 }}>{lead.business_name}</div>
                  <div style={{ fontSize: 11, color: "var(--color-text-secondary)", marginBottom: 8 }}>{lead.category} · {lead.city}</div>
                  {lead.phone && <div style={{ fontSize: 11, color: "var(--color-text-tertiary)", marginBottom: 8 }}>📞 {lead.phone}</div>}
                  <div style={{ display: "flex", gap: 6, flexWrap: "wrap" }}>
                    {lead.email1 && (
                      <button onClick={() => onEmail(lead)} style={{ background: "#EFF6FF", border: "none", borderRadius: 5, padding: "4px 8px", cursor: "pointer", fontSize: 11, color: "#3B82F6", fontWeight: 600 }}>
                        📧 Email
                      </button>
                    )}
                    <select value={lead.status} onChange={e => onUpdate(lead.id, "status", e.target.value)}
                      style={{ border: "0.5px solid var(--color-border-tertiary)", background: "var(--color-background-secondary)", borderRadius: 5, padding: "4px 8px", fontSize: 11, cursor: "pointer", color: "var(--color-text-secondary)" }}>
                      {columns.map(s => <option key={s}>{s}</option>)}
                    </select>
                  </div>
                </div>
              ))}
            </div>
          </div>
        );
      })}
    </div>
  );
}

function OutreachCard({ lead, onEmail, onUpdate }) {
  const socialScore = (lead.fb_followers || 0) + (lead.ig_followers || 0) + (lead.li_followers || 0);
  return (
    <div style={{ background: "var(--color-background-primary)", border: "0.5px solid var(--color-border-tertiary)", borderRadius: 14, padding: "18px 20px" }}>
      <div style={{ display: "flex", alignItems: "flex-start", gap: 16 }}>
        <div style={{ width: 46, height: 46, borderRadius: 12, background: "#EFF6FF", display: "flex", alignItems: "center", justifyContent: "center", fontWeight: 700, fontSize: 18, color: "#3B82F6", flexShrink: 0 }}>
          {lead.business_name[0]}
        </div>
        <div style={{ flex: 1 }}>
          <div style={{ display: "flex", alignItems: "center", gap: 10, marginBottom: 4 }}>
            <span style={{ fontWeight: 700, fontSize: 15, color: "var(--color-text-primary)" }}>{lead.business_name}</span>
            <StatusBadge status={lead.status} />
          </div>
          <div style={{ fontSize: 13, color: "var(--color-text-secondary)", marginBottom: 10 }}>
            {lead.category} · {lead.city} · {lead.reviews_count} reviews
            {socialScore > 0 && ` · ${socialScore.toLocaleString()} total followers`}
          </div>
          <div style={{ fontSize: 13, color: "#7C3AED", fontWeight: 600, marginBottom: 12 }}>
            🎯 Service: {lead.service_to_offer} &nbsp;|&nbsp; ⚠️ {lead.issues_found}
          </div>
          <div style={{ display: "flex", gap: 10, flexWrap: "wrap" }}>
            {lead.email1 && (
              <button onClick={onEmail} style={{ background: "#3B82F6", border: "none", borderRadius: 8, padding: "8px 16px", color: "#fff", fontWeight: 600, fontSize: 12, cursor: "pointer", display: "flex", alignItems: "center", gap: 6 }}>
                📧 Send Email
              </button>
            )}
            {lead.whatsapp || lead.phone ? (
              <button onClick={() => window.open(`https://wa.me/${lead.phone?.replace(/[^0-9]/g, "")}`)}
                style={{ background: "#ECFDF5", border: "1px solid #6EE7B7", borderRadius: 8, padding: "8px 16px", color: "#065F46", fontWeight: 600, fontSize: 12, cursor: "pointer" }}>
                💬 WhatsApp
              </button>
            ) : null}
            {lead.facebook_url && (
              <button onClick={() => window.open(lead.facebook_url)} style={{ background: "#EEF2FF", border: "1px solid #C7D2FE", borderRadius: 8, padding: "8px 16px", color: "#3730A3", fontWeight: 600, fontSize: 12, cursor: "pointer" }}>
                👥 Facebook
              </button>
            )}
            {lead.instagram_url && (
              <button onClick={() => window.open(lead.instagram_url)} style={{ background: "#FDF2F8", border: "1px solid #F9A8D4", borderRadius: 8, padding: "8px 16px", color: "#9D174D", fontWeight: 600, fontSize: 12, cursor: "pointer" }}>
                📸 Instagram
              </button>
            )}
            {lead.linkedin_url && (
              <button onClick={() => window.open(lead.linkedin_url)} style={{ background: "#EFF6FF", border: "1px solid #93C5FD", borderRadius: 8, padding: "8px 16px", color: "#1E40AF", fontWeight: 600, fontSize: 12, cursor: "pointer" }}>
                💼 LinkedIn
              </button>
            )}
          </div>
        </div>
      </div>
    </div>
  );
}

function Modal({ title, onClose, children }) {
  return (
    <div style={{ position: "fixed", inset: 0, background: "rgba(0,0,0,0.5)", zIndex: 1000, display: "flex", alignItems: "center", justifyContent: "center", padding: 20 }}>
      <div style={{ background: "var(--color-background-primary)", borderRadius: 16, padding: "24px", width: "100%", maxWidth: 560, maxHeight: "90vh", overflowY: "auto", border: "0.5px solid var(--color-border-tertiary)" }}>
        <div style={{ display: "flex", justifyContent: "space-between", alignItems: "center", marginBottom: 20 }}>
          <h2 style={{ margin: 0, fontSize: 18, fontWeight: 700, color: "var(--color-text-primary)" }}>{title}</h2>
          <button onClick={onClose} style={{ background: "var(--color-background-secondary)", border: "none", borderRadius: 8, width: 32, height: 32, cursor: "pointer", fontSize: 16, color: "var(--color-text-secondary)", display: "flex", alignItems: "center", justifyContent: "center" }}>✕</button>
        </div>
        {children}
      </div>
    </div>
  );
}

function AddLeadModal({ onClose, onAdd }) {
  const [form, setForm] = useState({ business_name: "", category: "", city: "", phone: "", email1: "", service_to_offer: "", address: "", area: "", state: "", reviews_count: 0, website_url: "", facebook_url: "", instagram_url: "", linkedin_url: "", issues_found: "", notes: "", date: new Date().toISOString().split("T")[0] });
  const set = (k, v) => setForm(p => ({ ...p, [k]: v }));
  return (
    <Modal title="➕ Add New Lead" onClose={onClose}>
      <div style={{ display: "grid", gridTemplateColumns: "1fr 1fr", gap: 12 }}>
        {[["Business Name", "business_name"], ["Category", "category"], ["City", "city"], ["Phone", "phone"], ["Email 1", "email1"], ["Service to Offer", "service_to_offer"], ["Address", "address"], ["Website", "website_url"], ["Facebook URL", "facebook_url"], ["Instagram URL", "instagram_url"], ["Issues Found", "issues_found"]].map(([label, key]) => (
          <div key={key} style={{ gridColumn: key === "address" || key === "issues_found" ? "1 / -1" : "auto" }}>
            <label style={{ fontSize: 11, fontWeight: 600, color: "var(--color-text-secondary)", display: "block", marginBottom: 5, textTransform: "uppercase", letterSpacing: "0.3px" }}>{label}</label>
            <input value={form[key]} onChange={e => set(key, e.target.value)}
              style={{ width: "100%", padding: "8px 10px", borderRadius: 7, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-secondary)", fontSize: 13, color: "var(--color-text-primary)", boxSizing: "border-box" }} />
          </div>
        ))}
        <div style={{ gridColumn: "1 / -1" }}>
          <label style={{ fontSize: 11, fontWeight: 600, color: "var(--color-text-secondary)", display: "block", marginBottom: 5, textTransform: "uppercase", letterSpacing: "0.3px" }}>Notes</label>
          <textarea value={form.notes} onChange={e => set("notes", e.target.value)} rows={3}
            style={{ width: "100%", padding: "8px 10px", borderRadius: 7, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-secondary)", fontSize: 13, color: "var(--color-text-primary)", resize: "vertical", boxSizing: "border-box", fontFamily: "inherit" }} />
        </div>
      </div>
      <div style={{ display: "flex", gap: 10, justifyContent: "flex-end", marginTop: 16 }}>
        <button onClick={onClose} style={{ padding: "9px 18px", borderRadius: 8, border: "0.5px solid var(--color-border-secondary)", background: "var(--color-background-primary)", cursor: "pointer", fontSize: 13, fontWeight: 600, color: "var(--color-text-primary)" }}>Cancel</button>
        <button onClick={() => form.business_name && onAdd(form)} style={{ padding: "9px 20px", borderRadius: 8, border: "none", background: "#3B82F6", color: "#fff", cursor: "pointer", fontSize: 13, fontWeight: 700 }}>✓ Add Lead</button>
      </div>
    </Modal>
  );
}
