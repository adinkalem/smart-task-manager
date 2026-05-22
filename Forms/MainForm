using System;
using System.ComponentModel;
using System.Drawing;
using System.Linq;
using System.Windows.Forms;

namespace SmartTaskManager
{
    public enum TaskPriority
    {
        Low,
        Medium,
        High
    }

    public class WorkTask
    {
        public Guid Id { get; set; }
        public string Title { get; set; }
        public string Description { get; set; }
        public DateTime DueDate { get; set; }
        public bool IsCompleted { get; set; }
        public TaskPriority Priority { get; set; }

        public WorkTask()
        {
            Id = Guid.NewGuid();
        }

        public WorkTask(string title, string description, DateTime dueDate, TaskPriority priority)
        {
            Id = Guid.NewGuid();
            Title = title;
            Description = description;
            DueDate = dueDate;
            Priority = priority;
            IsCompleted = false;
        }
    }

    public class MainForm : Form
    {
        private readonly BindingList<WorkTask> tasks = new BindingList<WorkTask>();
        private readonly BindingSource bindingSource = new BindingSource();

        private readonly Panel topPanel = new Panel();
        private readonly Panel leftPanel = new Panel();
        private readonly Panel rightPanel = new Panel();

        private readonly Label lblTitle = new Label();
        private readonly Label lblSubtitle = new Label();
        private readonly Label lblStats = new Label();

        private readonly DataGridView dgvTasks = new DataGridView();
        private readonly TextBox txtTitle = new TextBox();
        private readonly TextBox txtDescription = new TextBox();
        private readonly DateTimePicker dtpDueDate = new DateTimePicker();
        private readonly ComboBox cboPriority = new ComboBox();
        private readonly CheckBox chkCompleted = new CheckBox();
        private readonly TextBox txtSearch = new TextBox();

        private readonly Button btnAdd = new Button();
        private readonly Button btnUpdate = new Button();
        private readonly Button btnDelete = new Button();
        private readonly Button btnComplete = new Button();
        private readonly Button btnSearch = new Button();
        private readonly Button btnClear = new Button();

        private Guid selectedTaskId = Guid.Empty;

        public MainForm()
        {
            InitializeUi();
            SetupGrid();
            BindData();
            RefreshStatistics();
        }

        private void InitializeUi()
        {
            Text = "Smart Task Manager";
            StartPosition = FormStartPosition.CenterScreen;
            ClientSize = new Size(1180, 700);
            MinimumSize = new Size(1100, 650);
            Font = new Font("Segoe UI", 9F);
            BackColor = Color.FromArgb(245, 247, 250);

            topPanel.Dock = DockStyle.Top;
            topPanel.Height = 70;
            topPanel.BackColor = Color.FromArgb(34, 49, 63);

            lblTitle.Text = "Smart Task Manager";
            lblTitle.ForeColor = Color.White;
            lblTitle.Font = new Font("Segoe UI", 18F, FontStyle.Bold);
            lblTitle.AutoSize = true;
            lblTitle.Location = new Point(25, 12);

            lblSubtitle.Text = "Simple task organizer";
            lblSubtitle.ForeColor = Color.Gainsboro;
            lblSubtitle.Font = new Font("Segoe UI", 9F);
            lblSubtitle.AutoSize = true;
            lblSubtitle.Location = new Point(28, 43);

            topPanel.Controls.Add(lblTitle);
            topPanel.Controls.Add(lblSubtitle);

            leftPanel.Location = new Point(20, 90);
            leftPanel.Size = new Size(720, 560);
            leftPanel.BackColor = Color.White;
            leftPanel.Padding = new Padding(12);

            rightPanel.Location = new Point(760, 90);
            rightPanel.Size = new Size(390, 560);
            rightPanel.BackColor = Color.White;
            rightPanel.Padding = new Padding(15);

            Label lblGrid = new Label
            {
                Text = "Tasks",
                Font = new Font("Segoe UI", 11F, FontStyle.Bold),
                AutoSize = true,
                Location = new Point(12, 10)
            };

            dgvTasks.Location = new Point(12, 42);
            dgvTasks.Size = new Size(696, 506);
            dgvTasks.ReadOnly = true;
            dgvTasks.AllowUserToAddRows = false;
            dgvTasks.AllowUserToDeleteRows = false;
            dgvTasks.SelectionMode = DataGridViewSelectionMode.FullRowSelect;
            dgvTasks.MultiSelect = false;
            dgvTasks.AutoSizeColumnsMode = DataGridViewAutoSizeColumnsMode.Fill;
            dgvTasks.RowHeadersVisible = false;
            dgvTasks.BorderStyle = BorderStyle.None;
            dgvTasks.BackgroundColor = Color.White;
            dgvTasks.GridColor = Color.FromArgb(230, 230, 230);
            dgvTasks.EnableHeadersVisualStyles = false;
            dgvTasks.ColumnHeadersDefaultCellStyle.BackColor = Color.FromArgb(52, 73, 94);
            dgvTasks.ColumnHeadersDefaultCellStyle.ForeColor = Color.White;
            dgvTasks.ColumnHeadersDefaultCellStyle.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
            dgvTasks.DefaultCellStyle.SelectionBackColor = Color.FromArgb(210, 230, 255);
            dgvTasks.DefaultCellStyle.SelectionForeColor = Color.Black;
            dgvTasks.AlternatingRowsDefaultCellStyle.BackColor = Color.FromArgb(248, 249, 251);

            leftPanel.Controls.Add(lblGrid);
            leftPanel.Controls.Add(dgvTasks);

            AddLabel(rightPanel, "Task Details", 0, 0, 12, true);
            AddLabel(rightPanel, "Title", 0, 40);
            ConfigureTextBox(txtTitle, 0, 60, 355, 28);

            AddLabel(rightPanel, "Description", 0, 100);
            ConfigureTextBox(txtDescription, 0, 120, 355, 80, true);

            AddLabel(rightPanel, "Due Date", 0, 215);
            dtpDueDate.Location = new Point(0, 235);
            dtpDueDate.Size = new Size(355, 28);
            dtpDueDate.Format = DateTimePickerFormat.Short;

            AddLabel(rightPanel, "Priority", 0, 275);
            cboPriority.Location = new Point(0, 295);
            cboPriority.Size = new Size(355, 28);
            cboPriority.DropDownStyle = ComboBoxStyle.DropDownList;
            cboPriority.DataSource = Enum.GetValues(typeof(TaskPriority));

            chkCompleted.Text = "Completed";
            chkCompleted.Location = new Point(0, 335);
            chkCompleted.AutoSize = true;

            btnAdd.Text = "Add";
            btnUpdate.Text = "Update";
            btnDelete.Text = "Delete";
            btnComplete.Text = "Complete";
            btnSearch.Text = "Search";
            btnClear.Text = "Clear";

            StyleButton(btnAdd, Color.FromArgb(46, 204, 113), 0, 375);
            StyleButton(btnUpdate, Color.FromArgb(52, 152, 219), 120, 375);
            StyleButton(btnDelete, Color.FromArgb(231, 76, 60), 240, 375);
            StyleButton(btnComplete, Color.FromArgb(155, 89, 182), 0, 420);
            StyleButton(btnSearch, Color.FromArgb(52, 73, 94), 240, 465);
            StyleButton(btnClear, Color.FromArgb(127, 140, 141), 0, 510, 355);

            txtSearch.Location = new Point(0, 470);
            txtSearch.Size = new Size(225, 28);

            lblStats.Location = new Point(0, 530);
            lblStats.Size = new Size(355, 20);
            lblStats.Font = new Font("Segoe UI", 9F, FontStyle.Bold);

            rightPanel.Controls.AddRange(new Control[]
            {
                txtTitle, txtDescription, dtpDueDate, cboPriority, chkCompleted,
                btnAdd, btnUpdate, btnDelete, btnComplete, txtSearch, btnSearch, btnClear, lblStats
            });

            Controls.Add(leftPanel);
            Controls.Add(rightPanel);
            Controls.Add(topPanel);

            btnAdd.Click += btnAdd_Click;
            btnUpdate.Click += btnUpdate_Click;
            btnDelete.Click += btnDelete_Click;
            btnComplete.Click += btnComplete_Click;
            btnSearch.Click += btnSearch_Click;
            btnClear.Click += btnClear_Click;
            dgvTasks.SelectionChanged += dgvTasks_SelectionChanged;
        }

        private void AddLabel(Control parent, string text, int x, int y, int size = 9, bool bold = false)
        {
            parent.Controls.Add(new Label
            {
                Text = text,
                Location = new Point(x, y),
                AutoSize = true,
                Font = new Font("Segoe UI", size, bold ? FontStyle.Bold : FontStyle.Regular),
                ForeColor = Color.FromArgb(55, 55, 55)
            });
        }

        private void ConfigureTextBox(TextBox box, int x, int y, int width, int height, bool multiline = false)
        {
            box.Location = new Point(x, y);
            box.Size = new Size(width, height);
            box.BorderStyle = BorderStyle.FixedSingle;
            if (multiline)
                box.Multiline = true;
        }

        private void StyleButton(Button button, Color color, int x, int y, int width = 110)
        {
            button.Location = new Point(x, y);
            button.Size = new Size(width, 35);
            button.FlatStyle = FlatStyle.Flat;
            button.FlatAppearance.BorderSize = 0;
            button.BackColor = color;
            button.ForeColor = Color.White;
            button.Font = new Font("Segoe UI", 9F, FontStyle.Bold);
            button.Cursor = Cursors.Hand;
        }

        private void SetupGrid()
        {
            dgvTasks.AutoGenerateColumns = false;

            dgvTasks.Columns.Add(new DataGridViewTextBoxColumn { DataPropertyName = "Id", HeaderText = "ID", Visible = false });
            dgvTasks.Columns.Add(new DataGridViewTextBoxColumn { DataPropertyName = "Title", HeaderText = "Title" });
            dgvTasks.Columns.Add(new DataGridViewTextBoxColumn { DataPropertyName = "Description", HeaderText = "Description" });
            dgvTasks.Columns.Add(new DataGridViewTextBoxColumn { DataPropertyName = "DueDate", HeaderText = "Due Date" });
            dgvTasks.Columns.Add(new DataGridViewTextBoxColumn { DataPropertyName = "Priority", HeaderText = "Priority" });
            dgvTasks.Columns.Add(new DataGridViewCheckBoxColumn { DataPropertyName = "IsCompleted", HeaderText = "Done" });
        }

        private void BindData()
        {
            bindingSource.DataSource = tasks;
            dgvTasks.DataSource = bindingSource;
        }

        private void btnAdd_Click(object sender, EventArgs e)
        {
            if (string.IsNullOrWhiteSpace(txtTitle.Text))
            {
                MessageBox.Show("Title cannot be empty.");
                return;
            }

            tasks.Add(new WorkTask(
                txtTitle.Text.Trim(),
                txtDescription.Text.Trim(),
                dtpDueDate.Value.Date,
                (TaskPriority)cboPriority.SelectedItem
            ));

            bindingSource.ResetBindings(false);
            ClearInputs();
            RefreshStatistics();
        }

        private void btnUpdate_Click(object sender, EventArgs e)
        {
            WorkTask task = tasks.FirstOrDefault(t => t.Id == selectedTaskId);
            if (task == null)
            {
                MessageBox.Show("Select a task first.");
                return;
            }

            task.Title = txtTitle.Text.Trim();
            task.Description = txtDescription.Text.Trim();
            task.DueDate = dtpDueDate.Value.Date;
            task.Priority = (TaskPriority)cboPriority.SelectedItem;
            task.IsCompleted = chkCompleted.Checked;

            bindingSource.ResetBindings(false);
            RefreshStatistics();
        }

        private void btnDelete_Click(object sender, EventArgs e)
        {
            WorkTask task = tasks.FirstOrDefault(t => t.Id == selectedTaskId);
            if (task == null)
            {
                MessageBox.Show("Select a task first.");
                return;
            }

            if (MessageBox.Show("Delete selected task?", "Confirm", MessageBoxButtons.YesNo, MessageBoxIcon.Question) == DialogResult.Yes)
            {
                tasks.Remove(task);
                bindingSource.ResetBindings(false);
                ClearInputs();
                RefreshStatistics();
            }
        }

        private void btnComplete_Click(object sender, EventArgs e)
        {
            WorkTask task = tasks.FirstOrDefault(t => t.Id == selectedTaskId);
            if (task == null)
            {
                MessageBox.Show("Select a task first.");
                return;
            }

            task.IsCompleted = true;
            bindingSource.ResetBindings(false);
            RefreshStatistics();
        }

        private void btnSearch_Click(object sender, EventArgs e)
        {
            string keyword = txtSearch.Text.Trim();

            if (string.IsNullOrWhiteSpace(keyword))
            {
                dgvTasks.DataSource = bindingSource;
                return;
            }

            var filtered = new BindingList<WorkTask>(
                tasks.Where(t =>
                    (!string.IsNullOrEmpty(t.Title) && t.Title.IndexOf(keyword, StringComparison.OrdinalIgnoreCase) >= 0) ||
                    (!string.IsNullOrEmpty(t.Description) && t.Description.IndexOf(keyword, StringComparison.OrdinalIgnoreCase) >= 0))
                .ToList()
            );

            dgvTasks.DataSource = filtered;
        }

        private void btnClear_Click(object sender, EventArgs e)
        {
            ClearInputs();
            dgvTasks.DataSource = bindingSource;
            RefreshStatistics();
        }

        private void dgvTasks_SelectionChanged(object sender, EventArgs e)
        {
            if (dgvTasks.CurrentRow?.DataBoundItem is WorkTask task)
            {
                selectedTaskId = task.Id;
                txtTitle.Text = task.Title;
                txtDescription.Text = task.Description;
                dtpDueDate.Value = task.DueDate;
                cboPriority.SelectedItem = task.Priority;
                chkCompleted.Checked = task.IsCompleted;
            }
        }

        private void ClearInputs()
        {
            selectedTaskId = Guid.Empty;
            txtTitle.Clear();
            txtDescription.Clear();
            txtSearch.Clear();
            dtpDueDate.Value = DateTime.Today;
            cboPriority.SelectedIndex = 0;
            chkCompleted.Checked = false;
        }

        private void RefreshStatistics()
        {
            lblStats.Text = $"Total: {tasks.Count} | Completed: {tasks.Count(t => t.IsCompleted)} | Pending: {tasks.Count(t => !t.IsCompleted)}";
        }
    }
}
